
Some of the most common biometric technologies include the following:

- **Fingerprints**, which check the unique patterns of ridges and valleys on your fingertips using either optical, ultrasonic, or capacitive scanners. Fingerprint scanning has been broadly deployed within both Windows, using fingerprint scanners on laptops, and with Android and Apple devices that use fingerprint readers.
  
- **Retina scanning** uses the unique patterns of blood vessels in the retina to tell users apart.
  
- **Iris recognition** systems use pattern recognition and infrared imaging to uniquely identify an individual's eyes. Iris recognition can be accomplished from farther away than retina scans, making it preferable in many circumstances.
  
- **Facial recognition** techniques match specific features to an original image in a database. Facial recognition is widely used in Apple iPhone for FaceID, making it a broadly deployed biometric technology.
  
- **Voice recognition** systems rely on patterns, rhythms, and the sounds of a user's voice itself to recognize the user. 
  
- **Vein recognition**, sometimes called vein matching or vascular technology, uses scanners that can see the pattern of veins, often in a user's finger. Vein scanners do not need to touch the user, unlike fingerprint scanners, making them less likely to be influenced by things like dirt or skin conditions.
  
- **Gait analysis** measures how a person walks to identify them

Biometric technologies are assessed based on *four major measures*. The first is Type I errors, or the false rejection rate (**FRR**). False rejection errors mean that a legitimate biometric measure was presented and the system rejected it. 

Type II errors, or false acceptance errors, are measured as the false acceptance rate (**FAR**). These occur when a biometric factor is presented and is accepted when it shouldn't be. 

These are compared using a measure called the relative operating characteristic (**ROC**). The ROC compares the FRR against the FAR of a system, typically as a graph. For most systems, as you *decrease the likelihood of false rejection*, you *will increase the rate of false acceptance*, and determining where the accuracy of a system should be set to minimise false acceptance and prevent false rejection is an important element in the configuration of biometric systems.

>[!done] Evaluating Biometrics
>When you assess biometrics systems, knowing their FAR and FRR will help you determine their efficacy rates, or how effective they are at performing their desired function.
>
>The FIDO Alliance sets their *FRR* threshold for acceptance for certification for biometric factors to *3 in 100* attempts and at *1 in 10,000* for *FAR*.
>
>The Imposter Attack Presentation Match Rate (**IAPMR**), a measure that tackles the question of how often an attack will succeed. IAPMR is a challenging measure because it requires attacks that are designed to specifically take advantage of the weaknesses of any given biometric system.

