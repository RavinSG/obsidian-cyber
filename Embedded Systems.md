Embedded systems are often highly specialised, running customised operating systems and with very specific functions and interfaces that they expose to users. In a growing number of cases, however, they may embed a relatively capable system with Wi-Fi, cellular, or other wireless access that runs Linux or a similar, more familiar operating system.

Many embedded systems use a **real-time operating system** (RTOS). A RTOS is an operating system that is used when *priority needs to be placed on processing data as it comes in*, rather than using interrupts for the operating system or waiting for tasks being processed to be handled before data is processed. Since embedded systems are widely used for industrial processes where responses must be quick, real-time operating systems are used to *minimise the amount of variance* in how quickly the OS accepts data and handles tasks.

As a security professional, you need to be able to *assess embedded system security and identify way*s to ensure that they remain secure and usable for their intended purpose without causing the system itself to malfunction or suffer from unacceptable degradations in performance. 

Assessing embedded systems can be approached much as you would a traditional computer system:

1. *Identify the manufacturer* or type of embedded system and acquire documentation or other materials about it.
   
2. Determine how the embedded system *interfaces with the world*: does it connect to a network, to other embedded devices, or does it only have a keyboard or other physical interface?
   
3. If the device does provide a network connection, identify *any services or access to it provided through that network connection*, and how you can secure those services or the connection itself.
   
4. Learn about *how the device is updated*, if patches are available, and how and when those patches should be installed; then ensure a patching cycle is in place that matches the device's threat model and usage requirements.
   
5. *Document* what your organization would do *in the event that the device had a security issue* or compromise. Could you return to normal? What would happen if the device were taken offline due to that issue? Are there critical health, safety, or operational issues that might occur if the device failed or needed to be removed from service?
   
6. *Document your findings*, and ensure that appropriate practices are included in your organisation's operational procedures.

As more and more devices appear, this effort requires more and more time. Getting ahead of the process so that security is considered as part of the acquisitions process can help, but many devices may simply show up as part of other purchases, including things like the following:

- Medical systems, including devices found in hospitals and doctors' offices, may be network connected or have embedded systems. Medical devices like **pacemakers**, **insulin pumps**, and other external or implantable systems *can also be attacked* with exploits for pacemakers via Bluetooth already existing in the wild.
  
- **Smart meters** are deployed to track utility usage and bring with them a wireless control network managed by the utility. Since the *meters are now remotely accessible and controllable*, they provide a new attack surface that could interfere with power, water, or other utilities, or that could provide information about the facility or building.
  
- **Vehicles** ranging from cars to aircraft and even ships are now network connected, and frequently are *directly Internet connected*. If they are not properly secures, or if the backend *servers and infrastructure that support them are vulnerable*, attackers can take control . monitor, or otherwise seriously impact them.
 
- **Drones and autonomous vehicles** (AVs), as well as similar vehicles, *may be controlled from the Internet or through wireless command channels*. Encrypting their command-and-control channels and ensuring that they have appropriate controls if they are Internet or network connected are critical to their security.
  
- **VolP** systems include both backend servers as well as the VolP phones and devices that are deployed to desks and work locations throughout an organization. *The phones* themselves are a form of embedded system, with *an operating system that can be targeted* and may be vulnerable to attack. Some phones also provide interfaces that allow direct remote login or management, making them vulnerable to attack from VolP net-works. Segmenting networks to protect potentially vulnerable VolP devices, updating them regularly, and applying baseline security standards for the device help keep VolP systems secure.
  
- **Printers**, including multifunction printers (MFPs), frequently *have network connectivity built in*. Wireless and wired network interfaces provide direct access to the printers, and many printers have poor security models. Printers have been used as access points to protected networks, to reflect and amplify attacks, and as a means of gathering information. In fact, MFPs, copiers, and other devices that scan and *potentially store information to faxes, printouts, and copies* make these devices a potentially significant data leakage To in addition to the risk they can create as vulnerable networked devices that can act as reflectors and amplifiers in attacks, or as pivot points for attackers.
  
- **Surveillance systems** like camera systems and related devices that are used for security but that are also networked can provide attackers with a view of what is occurring inside a facility or organization. Cameras provide embedded interfaces that are commonly accessible via a web interface.