---
description: Harden TVA against Layer 2 compromise with HVR.
metaLinks:
  alternates:
    - https://app.gitbook.com/s/JfacbfboEQYUyRJCqjia/extensions/hvr-v2
---

# HVR

### Initial

HVR (short for Hashed Validation Reference) is an extension pattern to minimize the attack surface of the core pattern (TVA) - especially in the case of a compromise of the validation layer.

***

### Problem statement

If you use TVA as a core pattern, you have defined roles and tasks, as well as a very robust base, but also a clear problem: If the L2 (Validation Layer) is compromised, it can ultimately create fake requests and forward them to L3 (Execution Layer).\
\
The HVR pattern pursues the purpose of preventing exactly this.

***

### Pattern description

The HVR pattern incorporates an additional mechanism - or step - to protect against the validation layer acting as a single point of compromise. As soon as L1 initiates the request, it hashes it before passing it on to L2 (the validation layer). It then forwards this hash to the HVR add endpoint of L3.

L3 then adds this hash, along with a timestamp and an expiration date/time (e.g., timestamp +5 minutes), to the HVR table (a simple database or file).

Once L2 has received the actual request from L1, it can validate it and forward it to L3 if it has been successfully validated. L3 then hashes the request and checks whether it is in the HVR table and whether the expiration time has not yet been reached. Once the hash is found, it is of course immediately removed from the table, even before execution begins, to prevent recall attacks.

It is important to note that L1 can only communicate with L3 using the HVR add endpoint.

And, most importantly, L2 cannot communicate with L3’s HVR endpoint - without exception.&#x20;

The basic flow remains sequential, but with the add-on, L1 sends the HVR to L3 during initialization before the step of communicating with L2. (L1 -> L3; L1 -> L2 -> L3)

Below is a diagram illustrating the structure and flow of a request using the HVR pattern based on TVA:

<figure><img src="../.gitbook/assets/HVR_Pattern_Orbix_Labs.drawio.svg" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
The use of HVR improves the architecture, effectively “neutralizing” the standard BFT (Byzantine Fault Tolerance) formula. A page for BFT Reference is coming soon.
{% endhint %}
