
When adopting new security [[Policies]], [[standards]], and [[procedures]], *organisations should also provide a mechanism for exceptions to those rules*. Inevitably, unforeseen circumstances will arise that require a deviation from the requirements. 

The policy framework should lay out the *specific requirements for receiving an exception and the individual or committee with the authority to approve exceptions*.

The state of Washington uses an exception process that requires the requestor document the following information:

- Standard/requirement that requires an exception
- Reason for noncompliance with the requirement
- Business and/or technical justification for the exception
- Scope and duration of the exception
- Risks associated with the exception
- Description of any supplemental controls that mitigate the risks associated with the exception
- Plan for achieving compliance
- Identification of any unmitigated risks

Many *exception processes require the use of compensating controls* to mitigate the risk associated with exceptions to security standards. The Payment Card Industry Data Security Standard ([[Payment Card Industry Data Security Standard|PCI DSS]]) includes one of the most formal compensating control processes in use today. It sets out five criteria that must be met for a compensating control to be satisfactory:

- The control must meet the intent and rigor of the original requirement.
  
- The control must provide a similar level of defence as the original requirement, such that the compensating control sufficiently offsets the risk that the original PCI DSS requirement was designed to defend against.
  
- The control must be “above and beyond” other PCI DSS requirements.
  
- The control must address the additional risk imposed by not adhering to the PCI DSS requirement.
  
- The control must address the requirement currently and in the future.

For example, an organisation might find that it **needs to run an outdated version of an operating system** on a specific machine because the software necessary to run the business will only function on that operating system version. *Most security policies would prohibit* using the outdated operating system because it might be susceptible to security vulnerabilities. The organisation *could choose to run this system on an isolated network with either very little or no access to other systems* as a compensating control.

The general idea is that a *compensating control finds alternative means to achieve an objective* when the organisation cannot meet the original control requirement. Although PCI DSS offers a very formal process for compensating controls, the use of compensating controls is a common strategy in many different organisations, even those not subject to PCI DSS. Compensating controls balance the fact that it simply isn't possible to implement every required security control in every circumstance with the desire to manage risk to the greatest feasible degree.

In many cases, organisations adopt compensating controls to *address a temporary exception* to a security requirement. In those cases, the organisation should also develop remediation plans designed to bring the organization back into compliance with the letter and intent of the original control.