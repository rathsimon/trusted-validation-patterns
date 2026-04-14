---
description: Defining the fundament with the trusted validation architecture pattern.
metaLinks: {}
---

# TVA

### Initial

TVA (short for Trusted Validation Architecture) is a pattern designed to create a clearly defined and distinct system architecture with a focus on a validation layer.

***

### Problem statement

Modern systems are now almost always decentralized, which means that there is no one point that processes the complete request, or there is almost never only one system that is involved in the processing of a request - something that has a positive effect on the topic of security, scaling, etc.\
\
However, there is often a problem: unclear rules, which system takes over which task, duplications of validations, no fixed pattern.\
\
The TVA Pattern is deliberately simple and practical and does not reinvent the wheel, but documents a clear architecture.

***

### Pattern description

The TVA pattern basically consists of 3 levels, whereby these levels can of course contain several nodes each, this topic will be discussed in more detail in a separate section - which will appear in the next versions. In addition, it is important to me to mention that certain deviations may be quite useful, but the rough standard should be maintained when using the pattern.\
\
A sequence in the TVA pattern always goes through this sequentially, so L1 -> L2 -> L3.\
\
In addition, here is a diagram which represents the structure, as well as the process of a request:

<figure><img src="../.gitbook/assets/TVA_Diagram_Overview.svg" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
The use of the HVR pattern is not included on this page or in the diagram and the roles and task description below. Please note the corresponding page under "Extensions > HVR".
{% endhint %}

The different levels each have defined roles and tasks:

{% tabs %}
{% tab title="L1: Initiator Layer" %}
The Layer 1, also known as the Initiator Layer, has the fundamental task of receiving (or initializing) the request, performing basic validations and passing them on.

Defined tasks:

* Initialize or receive a request
* Initial validations, including whether the request contains the necessary parameters, or whether the user is generally allowed to call up the API endpoint

{% hint style="warning" %}
Important: First validations really only mean the basic validations that are necessary to reject malicious or incomplete requests directly. The actual validation, outside of auth and request standardizations must be performed in the layer 2 (validation layer).
{% endhint %}

* Standard/Static Rate-Limiting
* Forwarding the request to Layer 2
{% endtab %}

{% tab title="L2: Validation Layer" %}
The Layer 2, also known as the Validation Layer, has the fundamental task of validating the request, both technically and logically, and to forward the request to L3.

Defined tasks:

* Enhanced/Dynamic Rate-Limiting
* Validate the request for technical and logical validity
* Forwarding the request to Layer 3
{% endtab %}

{% tab title="L3: Execution Layer" %}
Layer 3, also known as the Execution Layer, has the fundamental task of executing the request.

Defined tasks:

* Execution of the request
{% endtab %}
{% endtabs %}
