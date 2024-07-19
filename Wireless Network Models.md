
Wireless technologies operate in one of four major connection models: **point-to-point**, **point-to-multipoint**, **mesh**, or **broadcast**. The figure below shows both a point-to-point network between two systems or devices, and a point-to-multipoint network design that connects to multiple devices from a single location.

![[P2P and P2Multi Wireless.png]]

Each of these design models is simple to understand. A point-to-point network connects two nodes, and transmissions between them can only be received by the endpoints. Point-to-multipoint networks like Wi-Fi have many nodes receiving the information sent by a node. Broadcast designs send out information on many nodes and typically do not care about receiving a response. GPS and radio are both examples of broadcast models.

### Attacks Against Wireless Networks and Devices

#### Evil Twin and Rouge Access Points

An evil twin is a malicious illegitimate access point that is set up to appear to be a legitimate, rusted network. The image below shows an evil twin attack where the *client wireless device has opted for the evil twin access point (AP) instead of the legitimate access point.* The attacker may have used a more powerful AP, placed the evil twin closer to the target, or used another technique to make the AP more likely to be the one the target will associate with.

![[Evil Twin Wireless.png|500]]

Once a client connects to the evil twin, the attacker will typically provide Internet connectivity so that the victim does not realise that something has gone wrong. The attacker will then *capture all of the victim's network traffic* and **look for sensitive data, passwords**, or other information that they can use. Presenting false versions of websites, particularly login screens, can provide attackers who have successfully implemented an evil twin with a quick way to capture credentials.

**Rogue access points** are APs added to your network either intentionally or unintentionally. Once they are connected to your network, they *can offer a point of entry to attackers* or other unwanted users. Since many devices have built-in wireless connectivity and may show up as an accessible network, it is important to monitor your network and facilities for rogue access points.

Most *modern enterprise wireless controller systems* have built-in functionality that allows them to *detect new access points* in areas where they are deployed. In addition, **wireless intrusion detection systems** or features can continuously scan for unknown access points and then determine if they are connected to your network by combining wireless network testing with wired network logs and traffic information. This helps separate out devices like mobile phones set up as hotspots and devices that may advertise a setup Wi-Fi network from devices that are plugged into your network and that may thus create a real threat.

#### Bluetooth Attacks

There are two common methods of Bluetooth attack: **bluejacking** and **bluesnarfing**. *Bluejacking sends unsolicited message*s to Bluetooth-enabled devices. *Bluesnarfing is unauthorised access to a Bluetooth device*, typically aimed at gathering information like contact lists or other details the device contains. Unfortunately, there aren't many security steps that can be put in place for most Bluetooth devices.

Many simply require pairing using an easily guessed code (often 0000), and then proceed to establish a *long-term key* that is used to secure their communications. Unfortunately, that long-term key is used to generate session keys when combined with other public factors, thus making attacks against them possible.

#### Bluetooth Impersonation Attacks

Bluetooth impersonation attacks (BIAs) take advantage of *weaknesses in the Bluetooth specification*, which means that all devices that implement Bluetooth as expected are likely to be vulnerable to them. They exploit a lack of **mutual authentication**, **authentication procedure downgrade options**, and the ability to **switch roles**.

Despite years of use of Bluetooth in everything from mobile devices to medical devices, wearables, and cars, the security model for *Bluetooth has not significantly improved*. Therefore, your best option to secure Bluetooth devices is to turn off Bluetooth if it is not absolutely needed and to leave it off except when in use. In addition, if devices allow a pairing code to be set, change it from the default pairing code and install all patches for Bluetooth devices.

#### RF and Protocol Attack

Attackers who want to conduct evil twin attacks, or who want systems to disconnect from a wireless network for any reason, have two primary options to help with that goal: **disassociation** attacks and **jamming**.

*Disassociation* describes what happens when a device disconnects from an access point. Many wireless attacks work better if the target system can be forced to disassociate from the access point that it is using when the attack starts. That will cause the system to attempt to reconnect, providing an attacker with a window of opportunity to set up a more powerful evil twin or to capture information as the system tries to reconnect.

The best way for attackers to force a system to disassociate is typically to *send a deauthentication frame*, a specific wireless protocol element that can be sent to the access point by spoofing the victim's wireless MAC address. When the AP receives it, it will disassociate the device, requiring it to then reconnect to continue. Since *management frames for networks that are using WPA2 are often not encrypted*, this type of *attack is relatively easy* to conduct. **WPA3**, however, *requires protected management frames* and will prevent this type of deauthentication attack from working.

Another means of attacking radio frequency networks like Wi-Fi and Bluetooth is to jam them. *Jamming will block all the traffic in the range or frequency* it is conducted against. Since jamming is essentially wireless interference, jamming may not always be intentional—in fact, running into devices that are sending out signals in the same frequency range as Wi-Fi devices isn't uncommon.