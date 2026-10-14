### Date Range: 10/5/26 - 10/9/26
---
__Instructor Goal:__
I will catch up on work I am missing or have incomplete by turning in at least 80% of missing assignments by the end of the week.

__Student Weekly Goal:__
Be ready for NASA HUNCH on friday and present well.

__Primary focus/topic:__
- Begin to implement splitting capstone into multiple c++ files and start to use header files
- Complete all needed hunch things
	- Brochure, Poster, Diagrams, Bussiness Cards

---
### Concepts
- Further understood appliances and why each of the ones we chose
	- VyOS - Router, Firewall, VPN
	- pfsaense - firewall
	- wazuh - HIDS
	- etc
- Installed and started to figure out how to use qt widgets to design the ui
	- Set up through vscode and individual designer application
	- Set up using nix flake so that configuration is declarative and can be ported to any device using nix
- Presented at NASA hunch and got lots of feedback on project
	- Need to do more research for a wholistic solution to the problem
	- Consider satellites in the plan
	- Need to do educational component based off of our solution.
---
### Challenges
- Setting up qt designer
	- In order to do this I had to set up both cmake and qt kits, and making them link was very difficult
	- Had to create custom kit that pulled all the needed ones and create a direnv to pull all nix path directories into a normal system
- Figuring out c++ header files
	- Header files are an unfamilliar concept coming from python so it takes a lot to wrap my head around
- Presenting at HUNCH PDR
	- Got better at it but at the beginning presentation was a little bit clunky
	- Was able to work out a plan with group
---
### Errors
- Lots of errors setting up qt
	- Often the designer just wouldn't open or cmake would freak out that it didn't have a kit selected
	- Ended up figuring it out
---
### Action plan for next week
- Refresh on notes from NASA hunch
- Finish deviding my current email code into seperate files to allow for more scability.
	- If I finish this I want to research how applications store persistent data
	- I also want to work on implementing oath2, which relies on above to store key