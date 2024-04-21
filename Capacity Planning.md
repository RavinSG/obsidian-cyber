
There are three main areas of capacity planning:

- **People**, where staffing and skillsets are necessary to deal with increased scale and disasters. Capacity planning for staff can be challenging since quickly staffing up in an emergency is hard. Instead, organisation s typically ensure that they have sufficient staff to ensure that appropriate coverage levels exist.
  
- **Technology** capacity planning focuses on understanding the technologies that an organisation has deployed and its ability to scale as needed. An example might include the capacity capabilities of a web server tool, a load balancer, or a storage device's throughput read/write rates.
  
- **Infrastructure**, where underlying systems and networks may need to scale. This can include network connectivity, throughput, storage, and any other element of infrastructure that may be needed to handle either changing loads or to support disaster recovery and [[Business Continuity]]. 

### Testing Resilience and Recovery Controls and Designs

Once you've implemented resilience and recovery controls, it is important to test and validate them. Listed below are methods in order of how much potential they have to disrupt an organisation's operations as part of the testing:

- **Tabletop exercise** use discussions between personnel assigned roles needed for the plan to validate the plan. This helps determine if there are missing components or processes. Tabletop exercises are the least potentially disruptive of the methods but also have the least connection to reality and may not detect issues that other methods would.
  
- **Simulation exercise** are drills or practices in which personnel simulate what they would do in an actual event. It is important to ensure that all staff know that the exercise is a simulation, as performed actual actions may cause disruption.
  
- **Parallel processing exercise** move processing to a hot site or alternate/backup system or facility to validate that the back up can perform as expected. This has the potential for disruption if the processing is not properly separated and the parallel system or site attempts to take over for the primary's data processing.
  
- **Failover exercise**s test full failover to an alternate site or system, and they have the greatest potential for disruption but also provide the greatest chance to fully test in a real-world scenario.