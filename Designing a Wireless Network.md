
Designing a [[Wi-Fi]] network for *usability*, *performance*, and *security* requires careful wireless access point (WAP) placement as well as configuration. Tuning and placement are critical, because wireless access points have a *limited number of channels* to operate within, and multiple *wireless access points using the same channel within range of each other can decrease the performance* and overall usability of the network. 

An important part of designing a wireless network is to conduct a **site survey**. Site surveys involve moving throughout the entire facility or space to determine what existing networks are in place and to look at the physical structure for the location options for your access points. In new construction, network design is often included in the overall design for the facility. Since most deployments are in existing structures, however, walking through a site to conduct a survey is critical.

Site survey tools test wireless signal strength as you walk, allowing you to match location using GPS and physically marking your position on a floor plan or map as you go. They then show where wireless signal is, how strong it is, and what channel or channels each access point or device is on in the form of a heat-map as shown below.

![[Wireless Heatmap.png]]

Determining which channels your access points will use is also part of this process. In the **2.4 GHz band**, each *channel is 20 MHz wide*, with a *5 MHz space between*. There are 11 channels for 2.4 GHz Wi-Fi deployments, resulting in overlap between channels in the 70 MHz of space allocated, as shown below.

![[2.4 GHz WiFi Channels.png]]

In most uses, this means that **channels 1, 6, and 11 are used** when it is possible to control channel usage in a space *to ensure that there is no overlap* and thus interference between channels. In dense urban areas or areas where other organisations may have existing Wi-Fi deployments, overlapping the channels in use onto your heat map will help determine what channel each access point should use.

The figure above shows the 2.4 GHz channels in use in **North America**. Additional channels are available in Japan, Indonesia, and outside of the United States, with those areas supporting channels 12 and 13 in addition to the 11 channels U.S. networks use.

Many access points will *automatically select the best channel* when they are deployed. Wireless network management software can monitor for interference and overlap problems and adjust your network using the same capabilities that they use to *determine if there are new rogue access points or other unexpected wireless devices* in their coverage area. These more advanced enterprise Wi-Fi controllers and management tools *can also adjust broadcast power* to avoid interference or even to overpower an unwanted device.

[[Controller and Access Point Security]]