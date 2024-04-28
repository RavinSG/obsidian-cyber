
Endpoints also face hardware vulnerabilities that require consideration in security designs. 

While *many hardware vulnerabilities are challenging or even impossible to deal with* directly, compensating controls can be leveraged to help ensure that the impact of them are limited.

**Firmware** attacks may occur through any path that allows access to the firmware. That includes through *executable updates*, *user downloads of malicious firmware* that appears to be legitimate, and even *remote network-enabled updates* for devices that provide networked access to their firmware or management tools.

Since firmware is embedded in the device, firmware vulnerabilities are a particular concern because *reinstalling an operating system or other software will not remove malicious firmware*.

An example of this is 2022's [MoonBounce](https://securelist.com/moonbounce-the-dark-side-of-uefi-firmware/105468/) malware, which is remotely installable and and targets a computer's Serial Peripheral Interface (**SPI**) flash memory. Once there, it will persist through reboots and reinstallation of the operating system. 

**End-of-life** or **legacy** hardware drives concerns around lack of support. Once a device or system has reached end-of-life, they typically will also reach the end of their support from the manufacturer.  *Without further updates* such as security fixes, you will be unable to address security problems directly and *will need compensating controls*.