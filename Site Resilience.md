**Resilience** is the ability to prepare for, respond to, and recover from disruptions. Organisations can design their systems to be resilient so that they can continue delivering services despite facing disruptions. An example is site resilience, which is used to ensure the availability of networks, data centres, or other infrastructure when a disruption happens. There are three types of recovery sites used for site resilience:

- *Hot sites*: A fully operational facility that is a duplicate of an organisation's primary environment. Hot sites can be activated immediately when an organisation's primary site experiences failure or disruption. Because of this, some organisations operate them full time, splitting traffic and load between multiple sites to ensure that the sites are performing properly. This approach also ensures that staff are in place in case of an emergency.

- *Warm sites*: Have some or all of the systems needed to perform the work required by the organization, but the live data is not in place. Warm sites are expensive to maintain because of the hardware costs, but they can reduce the total time to restoration because systems can be ready to go and mostly configured. They balance costs and capabilities between hot sites and cold sites.

- *Cold sites*: Have space, power, and often network connectivity, but they are **not prepared with systems or data**. This means that in a disaster an organization knows they would have a place to go but would have to bring or acquire systems. Cold sites are challenging because some disasters will prevent the acquisition of hardware, and data will have to be transported from another facility where it is stored in case of disaster. However, cold sites are also the least expensive option to maintain of the three types.

In each of these scenarios, the restoration order needs to be considered. Restoration order decisions *balance the criticality of systems and services* to the operation of the organization *against the need for other infrastructure to be in place and operational* to allow each component to be online, secure, and otherwise running properly.

A site restoration order might include a list like the following:

1. Restore network connectivity and a bastion or shell host.
2. Restore network security devices (firewalls, IPS).
3. Restore storage and database services.
4. Restore critical operational servers.
5. Restore logging and monitoring service.
6. Restore other services as possible.