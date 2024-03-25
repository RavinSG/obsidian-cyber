
### Test Categories

There are four major categories of penetration testing:

- *Physical penetration testing* focuses on identifying and exploiting vulnerabilities in an organisation's physical security controls. This can be breaking into buildings, bypassing access control systems, or compromising surveillance systems.

- *Offensive penetration testing* is a proactive approach where security professionals act as attackers to identify and exploit vulnerabilities in an organisation's networks systems, and applications. The goal is to simulate real-world cyber attacks and determine how well an organization can detect, respond, and recover from them.

- *Defensive penetration testing* focuses on evaluating an organisation's ability to defend against cyber attacks. Unlike offensive penetration testing which aims to exploit vulnerabilities, defensive penetration testing involves assessing the effectiveness of security policies, procedures, and technologies.

- *Integrated penetration testing* combines aspects of both offensive and defensive testing to provide a comprehensive assessment of an organisation's security posture. This approach involves close collaboration between offensive and defensive teams.

Once the type of assessment is known, one of the first things to decide about a penetration test is how much knowledge testers will have about the environment. Three typical classifications are used to describe this:

### Knowledge Classification

#### Known Environment Tests
Known environment tests are tests performed with full knowledge of the underlying technology, configurations, and settings that make up the target. Testers will typically have such information as *network diagrams, lists of systems and IP network ranges, and even credentials* to the systems they are testing. 

This means that a known environment test is often more complete, since testers can get to every system, service, or other target that is in scope, and will have credentials and other materials that will allow them to be tested. Of course, since testers can see everything inside an environment, *they may not provide an accurate view of what an external attacker would see*, and controls that would have been effective against most attackers may be bypassed.

#### Unknown Environment Tests
Unknown environment tests, are *intended to replicate what an attacker would encounter*. Testers are not provided with access to or information about an environment, and instead, they must gather information, discover vulnerabilities, and make their way through an infrastructure or systems like an attacker would. 

This approach can be *time-consuming*, but it can help provide a reasonably accurate assessment of how secure the target is against an attacker of similar or lesser skill. It is important to note that the *quality and skillset of your penetration tester or team is very important* when conducting a black-box penetration test—if the threat actor you expect to target your organization is more capable, a black-box tester can't provide you with a realistic view of what they could do.

#### Partially Known Environment
Partially known environment tests, are a blend of known and unknown environment testing. A partially known environment test may provide some information about the environment to the penetration testers *without giving full access, credentials, or configuration details*. A partially known environment test can *help focus penetration testers time and effort while also providing a more accurate view* of what an attacker would actually encounter.