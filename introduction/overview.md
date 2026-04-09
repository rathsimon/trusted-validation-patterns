---
description: Overview about this project and the structure of the patterns.
metaLinks:
  alternates:
    - https://app.gitbook.com/s/JfacbfboEQYUyRJCqjia/introduction/overview-v2
---

# Overview

This project focuses on defining patterns for systems with an emphasis on security and validation logic.

The entire project is structured as follows:

* **Core Pattern:** The core pattern, called TVA, is the heart and foundation of the system. It is kept simple and serves the purpose of defining the system’s architecture—that is, the basic structure, including the distribution of roles.
* **Extension Patterns:** As the name suggests, these patterns are extensions—additions to the existing core pattern.

The entire system has a modular structure, meaning the core pattern (the foundation) should be freely extensible, for example with VTEC, HVR, TTG, etc.

While one extension is never dependent on another, it usually makes sense to use them together, but it is not necessary.

Although the idea of the Core Pattern and the extensions is presented this way in this project, the extensions are of course potentially useful and implementable as standalone patterns even without the Core Pattern - so feel free to take as much liberty as possible here.
