Cybersecurity professionals configuring vulnerability scans should pay careful attention to the *configuration settings related to the scan sensitivity level*. These settings determine the types of checks that the scanner will perform and should be customised to ensure that the *scan meets its objectives while minimising the possibility of disrupting* the target environment.

Administrators may also improve the efficiency of their scans by *configuring the specific plug-ins that will run* during each scan. These plugins are often grouped into families based on the operating system, application, or device that they involve. Disabling unnecessary plugins improves the speed of the scan by bypassing unnecessary checks and also may *reduce the number of false positive results* detected by the scanner.

For example, an organization that does not use the Amazon Linux operating system may choose to disable all checks related to Amazon Linux in their scanning template.

> [!warning] Warning
> Some plug-ins perform tests that may actually *disrupt activity on a production system* or, in the worst case, *damage content on those systems*. Administrators want to run these scans because they may identify problems that could be exploited by a malicious source. At the same time, cybersecurity professionals clearly don't want to cause problems on the organisation's network and, as a result, may limit their scans to nonintrusive plug-ins.
> 
> One way around this problem is to *maintain a test environment* containing copies of the same systems running on the production network and running scans against those test systems first.
