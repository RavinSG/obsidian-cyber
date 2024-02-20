The Biba model is arguably the equivalent of the [[Bell-La Padula Model]] but for the [[Integrity]] of the CIA triad.

This model applies the rule to objects (data) and subjects (users) that can be summarised as "**no write up, no read down**". This rule means that subjects can create or write content to objects at or below their level but can only read the contents of objects above the subject's level.

| Advantages | Disadvantages |
| :--: | :--: |
| This model is simple to implement. | There will be many levels of access and objects. Things can be easily overlooked when applying security controls. |
| Resolves the limitations of the Bell-La Padula model by addressing both confidentiality and data integrity. | Often results in delays within a business. For example, a doctor would not be able to read the notes made by a nurse in a hospital with this model. |

The Biba model is used in organisations or situations *where integrity is more important than confidentiality*. For example, in software development, developers may only have access to the code that is necessary for their job. They may not need access to critical pieces of information such as databases, etc. 