Industrial and manufacturing systems are often described using one of two terms. **Industrial Controls Systems** (ICSs) is a broad term for industrial automation, and **Supervisory Control and Data Acquisition** (SCADA) often refers to large systems that run power and water distribution or other systems that cover large areas. 

**SCADA** is a type of system architecture that *combines data acquisition and control devices*, *computers*, *communications capabilities*, and *an interface to control and monitor* the entire architecture. SCADA systems are commonly found running complex manufacturing and industrial processes, where the ability to monitor, adjust, and control the entire process is critical to success.

Below a simplified overview of a SCADA system. You'll see that there are **remote telemetry units** (RTUs) that collect data from sensors and **programmable logic controllers** (PLCs) that control and collect data from industrial devices like machines or robots. Data is sent to the system control and monitoring controls, allowing operators to see what is going on and to manage the SCADA system.

![[SCADA System.png]]

Since ICS and SCADA systems *combine general-purpose computers* running commodity operating systems *with industrial devices with embedded systems and sensors*, they present a complex security profile for security professionals to assess.

In many cases, they must be *addressed as individual components* to identify their unique security needs, including things like **customised industrial communication protocols** and **proprietary interfaces**.

A key thing to remember when securing complex systems like this is that *they are often designed without security in mind*. That means that *adding security may interfere with their function or* that security devices *may not be practical* to add to the environment. In some cases, isolating and protecting ICS, SCADA, and embedded systems is one of the most effective security models that you can adopt.