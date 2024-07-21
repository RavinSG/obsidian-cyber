
Organisations use a wide variety of mobile devices, ranging from phones and tablets to more specialised devices. As you consider how your organisation should handle them, you *need to plan for your deployment and management model*, whether you will use a mobile device management tool, and what security options and settings you will put in place.

### Mobile Device Deployment Methods

When organisations use mobile devices, one important design decision is the deployment and management model that will be selected. The most common options are **BYOD**, or bring your own device; **CYOD**, or choose your own device; **COPE**, or corporate-owned, personally enabled; and **fully corporate owned**.

Each of these options has advantages and disadvantages, as outlined below.

| Type                                                | Who owns the device | Who controls and <br>maintains the device | Description                                                                                                                                                                                                                     |
| --------------------------------------------------- | ------------------- | ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **BYOD**<br>Bring Your Own Device                   | The user            | The user                                  | The user brings their own personally <br>owned device. This provides more user freedom and lower cost to the <br>organization, but greater risk since the organization does not control, secure, <br>or manage the device. <br> |
| **CYOD**<br>Choose your own device                  | The organisation    | The organisation                          | The organization owns and maintains the device, but allows the user to <br>select it. <br>                                                                                                                                      |
| **COPE**<br>Corporate-owned, personally enabled<br> | The organisation    | The organisation                          | Corporate-provided devices allow reasonable personal use while meeting enterprise security and control needs.                                                                                                                   |
| **Corporate-owned**                                 | The organisation    | The organisation                          | Corporate-owned provides the greatest control but least flexibility.                                                                                                                                                            |

These options boil down to a few common questions. First, who owns, chooses, and pays for the device and its connectivity plans? Second, how is the device managed and supported? Third, how are data and applications managed, secured, and protected?

**BYOD** places the control in the hands of the end user since they select and manage their own device. In some BYOD models, the *organisation may use limited management capabilities*, such as the *ability to remotely wipe email or specific applications*, but BYOD's control and management model is heavily based on the user. This option provides *far less security and oversight* for the organization.

In **CYOD** models, the organization pays for the device and typically for the cellular plan or other connectivity. The user selects the device, sometimes from a list of preferred options, rather than bringing whatever they would like to use. In a CYOD design of this type, *support is easier* since only a *limited number of device types* will be encountered, and that *can make a security model easier to establish* as well. Since CYOD continues to leave the device in the hands of the user, security and management is likely to remain less standardised, although this can vary.

In a **COPE** model, the device is company-owned and managed. COPE recognises that users are unlikely to want to carry two phones and thus allows reasonable personal use on corporate devices. This model *allows the organization to control the device more fully* while still allowing personal use.

A fully **corporate-owned** and -managed device is the most controlled environment and frequently more closely resembles corporate PCs with a complete control and management suite. This is the *least user-friendly* of the options since a corporate-chosen and -managed device will meet corporate needs but frequently lacks the flexibility of the more end user–centric designs.

One key technology that can help make mobile device deployments more secure is the use of virtual desktop infrastructure ([[Storage Resiliency#VDI|VDI]]) to allow relatively low-security devices to access a secured, managed environment. Using VDI *allows device users to connect to the remote environment, perform actions, and then return to normal use of their device*. Containerisation tools can also help split devices between work and personal-use environments, allowing a work container or a personal container to be run on a device without mixing data and access.

### [[Hardening Mobile Devices]]

### [[Mobile Device Management]]