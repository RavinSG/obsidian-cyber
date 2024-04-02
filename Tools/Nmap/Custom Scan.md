
If you want to experiment with a new TCP flag combination beyond the built-in TCP scan types, you can do so using `--scanflags`. For instance, if you want to set **SYN**, **RST**, and **FIN** simultaneously, you can do so using `--scanflags RSTSYNFIN`. As shown in the figure below, if you develop your custom scan, you need to know how the different ports will behave to interpret the results in different scenarios correctly.

![[Custom Scan.png]]

Finally, it is essential to note that the ACK scan and the window scan were very efficient at helping us *map out the firewall rules*. However, it is vital to remember that just because a firewall is not blocking a specific port, **it does not necessarily mean that a service is listening on that port**. 

For example, there is a possibility that the firewall rules need to be updated to reflect recent service changes. Hence, **ACK and window scans are exposing the firewall rules, not the services**.

