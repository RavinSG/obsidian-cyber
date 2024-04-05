
### Code Signing

Code signing provides developers with a way to *confirm the authenticity of their code* to end users. Developers use a cryptographic function to digitally sign their code with their own private key and then browsers can use the developer’s public key to verify that signature and ensure that the code is legitimate and was not modified by  unauthorised individuals.

### Code Reuse

Many organisations reuse code not only internally but by making use of third-party software libraries and software development kits (SDKs). Organisations trying to make libraries more accessible to developers often publish software development kits (SDKs).

Organisations may also introduce third-party code into their environments when they outsource code development to other organisations. Security teams should ensure that *outsourced code is subjected to the same level of testing as internally developed code*.

### Software Diversity

Security professionals seek to *avoid single points of failure* in their environments to avoid availability risks if an issue arises with a single component. Security professionals should watch for places in the organisation that are dependent on a single piece of source code, binary executable files, or compilers. Though it *may not be possible to eliminate all* of these dependencies, *tracking them is a critical part* of maintaining a secure codebase.

### Code Repositories

Code repositories are *centralised locations for the storage and management of application source code*. The main purpose of a code repository is to store the source files used in software development in a centralised location that allows for secure storage and the coordination of changes among multiple developers.

Code repositories also perform *version control*, allowing the tracking of changes and the rollback of code to earlier versions when required. By exposing code to all developers in an organization, code repositories *promote code reuse*. They also help *avoid the problem of dead code*, where code is in use in an organization but nobody is responsible for the maintenance of that code

### Integrity Measurement

Cybersecurity teams should also work hand in hand with developers and operations teams to ensure that *applications are provisioned and de-provisioned in a secure manner* through the organisation’s approved release management process. This process should include code integrity measurement. Code integrity measurement uses cryptographic hash functions to verify that the *code being released into production matches the code that was previously approved*.

### Application Resilience

When we design applications, we should create them in a manner that makes them resilient in the face of changing demand. We do this through the application of two related principles:

- **Scalability** says that applications should be designed so that computing resources they require may be incrementally added to support increasing demand.

- **Elasticity** goes a step further than scalability and says that applications should be able to *automatically provision resources* to scale when necessary and then *automatically de-provision those resources* to reduce capacity (and cost) when it is no longer needed.