### Date Range: 10/19/26 - 10/23/26
---
__Instructor Goal:__
I will improve upon my Python coding skills by collaborating with classmates on labs this week and answering both their and my instructors questions.

__Student Weekly Goal:__
I will learn how Oauth2.0 works and how to implement it in c++, I will also figure out what I need to work on for NASA Hunch for the next part of out project after the PDR.

__Primary focus/topic:__
- Continue to implement splitting capstone into multiple c++ files and start to use header files
- Refocus on NASA Hunch
- Pay close attention to web development lessons as that is my weakest point

---
### Concepts
- CSS
	- I got a lot more comfortable with css, and I feel like I better understand what I need to do to accomplish something, even if that is just knowing what I need to look up to get the correct answer.
	- Further understood css syntax and how to style each element individually vs a type as a whole
- Python
	- Learned about python classes in PCAP modules
	- I already know most of it, however I did not know about the build in attributes of every class like \_\_dict\_\_, which is a built in dictionary that stores all methods in a class
- Large projects
	- As my capstone project gets larger I am better understanding how to split jobs between multiple files and how to use those to create something better as a whole than one enormous spaghetti code file
- Hunch Plan
	- We have planned out what we as a group want to keep working on for NASA hunch
	- Specifically I need to better expand out simulation to include vpn/encryption and wireless radio tower communication
---
### Challenges
- Going into the web styling project I had very little knowledge of CSS, so it was a fun challenge to attempt to figure out what I needed to do and with help from classmates and google I was able to figure it out
- Planning how to structure capstone project
	- There are infinite ways to structure each coding project so I want to ensure that what I choose now is scale-able and I wont have to go back and change everything later.
- Ensuring I configure NASA HUNCH router correctly
	- Had to read VyOS documentation and compare what I decided to do with other peoples examples
---
### Errors
- Defining enum class in c++ struct
	- Enums in c_++ are defined as classes, as such they cannot be direct member variables of a struct
	- I had to declare local class and different variable of the type enum class to be able to store the value
- Packets wouldn't send in gns3 how I wanted them to be able to test encryption
	- I realized that I was using a simulated computer to test that I had specifically fire walled to not allow data to be sent where I wanted to go
	- Ended up setting up a management vlan that has no restricted permissions for testing
---
### Action plan for next week
- Capstone
	- Be able to send basic email through refactored capstone project
	- Begin to implement oath2.0 or sqlite
- HUNCH
	- Continue to configure router and get basic test data for routing
	- Begin researching how to test everything
	- Explore dhcp and other permissions so people cannot just change their ip address to get elevated permissions.
- Finish both next tech+ module and current PCAP module