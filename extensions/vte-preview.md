---
description: Validating secret and token retrieval with VTE.
metaLinks:
  alternates:
    - https://app.gitbook.com/s/JfacbfboEQYUyRJCqjia/extensions/vte-v2
---

# VTE (preview)

### Initial

VTE (short for Validation Token Exchange) is an extension pattern that allows the output of secrets only via the validation layer and in the process of validation.

***

### Problem statement

Secrets are often stored in environment variables across countless systems; using Key Vaults and other services can prevent this and ensure that secrets are at least managed centrally.

However, there is one major problem: As a rule, only the identity is used as a criterion for retrieving secrets. The system does not evaluate what it does with the secret - or whether the resulting transaction makes sense.&#x20;

***

### Pattern description

The foundation is provided by the implementation of the TVA pattern - or at least Layer 2, i.e., the validation layer. This layer is specifically designed to assess the following:\
Should this system be able to retrieve the secret based on the information L2 has, specifically for the intended transaction? And if so, is it possible and appropriate to set a scope and/or a time limit for this secret?

For example, System X wants to request a DB access token to transfer €20,000 from Person 1’s account to Person 2.

System X can now request the DB access token using the predefined “transaction” function. This transaction must always include specific parameters. In the background, the validation layer checks whether the parameters and circumstances make logical sense. For example, if Person 1’s account balance is less than €20,000 and they do not have an overdraft limit, the secret retrieval is rejected - because the transaction would not be logical.

If the transaction is logical and the function could be validated, then the token might be limited to exactly this table or prepared statement and be valid for only 5 minutes.

{% hint style="info" %}
This is just a simple example for illustrative purposes; actual applications can vary greatly and may require different levels of validation or only superficial validation.
{% endhint %}

The picture below shows the architectural structure of the VTE pattern:

<figure><img src="../.gitbook/assets/VTE_Diagram_Overview.svg" alt=""><figcaption></figcaption></figure>

In summary, the purpose of this pattern is to link the disclosure of secrets to logical validations—rather than simply verifying the correct identity - and to enhance security through the dynamic and context-dependent disclosure of secrets with appropriate attributes.
