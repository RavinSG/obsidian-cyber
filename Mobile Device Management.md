
Mobile devices can be a challenge to manage, particularly due to operating system limitations, variability between hardware manufacturers, carrier settings, and operating system versions.

When you add in the wide variety of device deployment models, security practitioners face real challenges in an increasingly mobile device–focused environment.
Thus, when administrators and security professionals need to manage mobile devices, they frequently turn to **mobile device management** (MDM) or **unified endpoint management** (UEM) tools. 

*MDM* tools specifically *target devices like Android and iOS phones, tablets*, and other similar systems. 

*UEM* tools *combine mobile devices, desktops and laptops*, and many other types of devices in a single management platform.

Regardless of the type of tool you choose, there are a number of features your organization may use to ensure that your mobile devices and the data they contain are secure. The following list contains some feature available in MDM, UEM, and mobile application management (MAM) tools. 

- *Application management* features are important to allow enterprise **control of applications**. These features may include deploying specific applications to all devices; limiting which applications can be installed; remotely adding, removing, or changing applications and settings for them; or monitoring application usage.
  
- *Content management* (sometimes called MCM, or mobile content management) ensures **secure access and control of organisational files**, including documents and media on mobile devices. A major concern for mobile device deployments is the combination of organisational data and personal data on BYOD and shared-use devices. Content management features *lock away business data in a controlled space* and then help manage access to that data. In many cases, this requires use of the MDM's application on the mobile device to access and use the data.

- *Remote-wipe capabilities* are used when a device is lost or stolen or when the owner is no longer employed by the organization. It is important to understand the difference between a full device wipe and wiping tools that can wipe only the organisational data and applications that have been deployed to the device. In environments where individuals own the devices, **remote wipe can create liability** and other issues if it is used and wipes the device. At the same time, remote wipe with a confirmation process that lets you know when it has succeeded is a big part of helping protect organisational data.
  
- *Geolocation and geofencing* capabilities allow you to **use the location of the phone to make decisions** about its operation. Some organisations may only allow corporate tablets to be used inside corporate facilities to reduce the likelihood of theft or data access outside their buildings. Other organisations may want devices to wipe themselves if they leave a known area. Geolocation can *also help locate lost devices*, in addition to the many uses for geolocation that we are used to in our daily lives with mapping and similar tools.
  
- *Screen locks, passwords, and PINs* are all part of normal device security models to prevent unauthorised access. Screen lock time settings are one of the most frequently set security options for basic mobile device security. Much like desktops and laptops, mobile device management tools also set things like password length, complexity, and how often passwords or PINs must be changed.
  
- *Biometrics* are widely available on modern devices, with fingerprints and facial recognition the most broadly adopted and deployed. Biometrics can be integrated into mobile device management capabilities so that you can deploy biometric authentication for users to specific devices and leverage biometric factors for additional security or ease of use.
  
- *Context-aware authentication* goes beyond PINs, passwords, and biometrics to better reflect user behaviour. Context may include things like **location**, **hours of use**, and a wide range of other behavioural elements that can determine whether a user should be able to log in.
  
- *Containerisation* is an increasingly common solution to handling separation of work and personal-use contexts on devices. Using a secure container to run applications, store data, and otherwise keep the use of a device separate greatly reduces the risk of cross-contamination and exposure. In many MDM models, **applications use wrappers** to run them, helping keep them separate and secure. In others, a complete containerisation environment is run as needed.
  
- *Storage segmentation* can be used to keep personal and business data separate as well. This may be separate volumes or even **separate encrypted volumes** that require specific applications, wrappers, or containers to access them. In fact, *storage segmentation and containerisation or wrapper technology are often combined* to better implement application and separation.
  
- *Full-device encryption* (FDE) remains the best way to ensure that stolen or lost devices don't result in a data breach. When combined with remote-wipe capabilities and strong authentication requirements, FDE can provide the greatest chance of a device resisting data theft.
  
- *Push notifications* may seem like an odd inclusion here, but sending messages to devices can be useful in a number of scenarios. You may need to **alert a user** to an issue or ask them to perform an action. Or you may want to communicate with someone who found a lost device or tell a thief that the device is being tracked! Thus, having the ability to send messages from a central location can be a useful tool in an MDM or UEM system.

MDM and UEM tools also provide a rich set of controls for user behaviors. They can *enable closed or managed third-party application stores* or limit what your users can download and use from the application stores that are native to the operating system or device you have deployed. They can also *monitor for firmware updates and versions*, including whether firmware over-the-air (OTA) updates have been applied to ensure that patching occurs.

Of course, users may try to *get around* those controls *by rooting* their devices, *or jailbreaking* them so that they can sideload (manually install from a microSD card or via a USB cable) programs or even a custom firmware on the device. **MDM and UEM tools will detect these activities** by checking for known good firmware and software, and they can apply allow or block lists to the applications that the devices have installed.

MDM and UEM tools also typically allow administrators to **control GPS tagging** for photos and other documents that may be able to embed GPS data about where they were taken or created. The ability to use *location data can be a useful privacy control* or may be required by the organization as part of documentation processes.

Administrators may also want to control how devices use their wireless connectivity. That can take the form of limiting which Wi-Fi networks devices can connect to, *preventing them from forming or joining ad hoc wireless networks*, and disabling tethering and the ability to become a wireless hotspot. Bluetooth and NFC controls can also help prevent the device from being used in ways that don't fit organisational security models, such as use as a payment method or access device.