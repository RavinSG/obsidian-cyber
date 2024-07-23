
The first step toward a mature incident response capability for most organisations is to *understand the incident response process* and what happens at each stage. Organisations must prepare for incidents, identify incidents when they occur, and then contain and remove the artefacts of the incidents. *Once the incident has been contained*, the organization can work to *recover and return to norma*l, and then make sure that the lessons learned from the incident are baked into the preparation for the next time something occurs.

![[Incident Response Life cycle.png]]

The figure above shows the six steps of the [[Incident Response Lifecycle|incident response process]].

1. **Preparation**. In this phase, you *build the tools, processes, and procedures* to respond to an incident. That includes building and training an incident response team, conducting exercises, documenting what you will do and how you will respond, and acquiring, configuring, and operating security tools and incident response capabilities.
   
2. **Detection**. This phase involves *reviewing events to identify incidents*. You must pay attention to indicators of compromise, use log analysis and security monitoring capabilities, and have a comprehensive awareness and reporting program for your staff.
   
3. **Analysis**. Once an event has been identified as potentially being part of an incident, it needs to be analysed. That includes *identifying other related events* and what *their target or impact* is or was.
   
4. **Containment**. Once an incident has been identified, the incident response team needs to contain it to *prevent further issues or damage*. Containment can be challenging and may not be complete if elements of the incident are not identified in the initial identification efforts. This can involve *quarantine*, which places a system or device in an isolated network zone or removes it from a network to ensure that it cannot impact other devices.
   
5. **Eradication**. The eradication stage involves *removing the artefacts associated* with the incident. In many cases, that will involve *rebuilding or restoring systems* and applications from backups rather than simply removing tools from a system since proving that a system has been fully cleaned can be very difficult. Complete eradication and verification is crucial to ensuring that an incident is over.
   
6. **Recovery**. Restoration to normal is the heart of the recovery phase. That may mean *bringing systems or services back online* or other actions that are part of a return to operations. Recovery requires eradication to be successful, but it also involves implementing fixes to ensure that whatever security weakness, flaw, or action that allowed the incident to occur has been remediated to prevent the event from immediately reoccurring.

In addition to these six steps, organisations typically conduct a **lessons learned session**. These sessions are important to ensure that organisations improve and do not make the same mistakes again. They may be as simple as patching systems or as complex as needing to redesign permission structures and operational procedures. Lessons learned are then used to inform the preparation process, and the cycle continues.

Although this list may make it appear as if incidents always proceed in a linear fashion from item to item, *many incidents will move back and forth between stages* **as additional discoveries are made or as additional actions are taken by malicious actors**. So, you need to remain nimble and understand that you may not be in the phase you think you are, or that you need to operate in multiple phases at once as you deal with components of an incident—or multiple incidents at once!

- [[Preparing for Incident Response]]