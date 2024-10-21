
These tools are those *intended to capture information about attackers and their techniques* and to *disrupt ongoing attacks*. Capturing information about attackers can provide defenders with useful details about their tools and processes and can help defenders build better and more secure networks based on real-world experiences. There are four major types of information gathering tools. 

### Honeypots

The first and most common is the use of honeypots. Honeypots are systems that are *intentionally configured to appear to be vulnerable* but that are **actually heavily instrumented and monitored systems** that will document everything an attacker does while retaining copies of every file and command they use. They appear to be legitimate and may have tempting false information available on them.

### Honeynets

Much like honeypots, honeynets are networks set up and instrumented to collect information about network attacks. In essence, *a honeynet is a group of honeypots* set up to be even more convincing and to provide greater detail on attacker tools due to the variety of systems and techniques required to make it through the network of systems.

### Honeyfiles

Unlike honeynets and honeypots, which are used for adversarial research, honeyfiles are *used for intrusion detection*. A honeyfile is an intentionally attractive file that contains unique, detectable data that is left in an area that an attacker is likely to visit if they succeed in their attacks. If the data contained in a honeyfile is *detected leaving the network, or is later discovered outside of the network*, the organisation knows that the *system was breached*.

### Honeytokens

Honeytokens are data that is intended to be attractive to attackers but which is used specifically to allow security professionals to track data. They may be entries in databases, files, directories, or any other data asset that can be specifically identified. IDS, IPS, DLP, and other systems are then configured to watch for honeytokens that should not be sent outside the organisation or access under normal circumstances because they are not actual organisational data.

