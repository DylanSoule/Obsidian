### Date Range: 11/1/26 - 11/6/26
---
__Instructor Goal:__
None

__Student Weekly Goal:__
Work to improve my personal website and finish certification modules so I will have more time to study for certification tests and work on other projects.

__Primary focus/topic:__
- PCAP module 4 (misc.)
- Tech+ module 5+6 (Cybersecurity and applications)
-  Website development

---
### Concepts
- Web development
	- Foundations of JS and how to incorporate it with html and css for dynamic pages
	- How to better style html
	- Organized directory structure
- HUNCH
	- I worked through creating an IPSEC VPN for my NASA hunch simulation and got encryption on the bridge between routers that simulate the earth space link working
- Capstone
	- I refactored a little bit the easy interface to have the multi interface hold the handles and their associated messages so that when the prepare function exits the data associated with the handle still exists
	- I did this using c++ smart pointers and an unordered list to preserve the libcurl raw pointers and not do unnecessary copying of large string variables
---
### Challenges
- VPN configuration
	- Routers wouldn't boot
		- After lots of configuration debugging and systemd debugging I figured out I just needed to increase the allocated RAM to virtual machines
	- Traffic was only being encrypted in one direction
		- I had an old static next-hop configuration in one of the routers that was taking precedence over the vpn
		- I just had to delete this configuration to get it working
- Personal website
	- I wanted to do a complex thing for my personal website
		- Create a file directory for my portfolio to make navigation a cool and fun experience
		- I still am not entirely sure how to do this but it lead me to learning about dynamic html generation through javascript
---
### Errors
- CSS styling wouldn't apply
	- I had the file path named wrong
	- There were general styling in the style sheet that over road other things that I was trying to do
---
### Action plan for next week
- I want to continue working on my personal website next week, and keep channeling my stoke about it toward it in a productive way.
- Work in simulating 4g network for nasa HUNCH
- Study for PCAP certification for the week after