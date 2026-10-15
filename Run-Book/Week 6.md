### Date Range: 9/21/26 - 9/25/26
---
__Instructor Goal:__
I will start work on my personal project by having my first deliverable finished or close to finished by the end of the week.

__Student Weekly Goal:__
Complete HUNCH poster and figure out how to receive emails through libcurl in c++

__Primary focus/topic:__
- Personal Project: Receive emails through c++ using libcurl(or another library if libcurl is unable to do it)
- Hunch: Finish poster, and arrange topology to achieve what is required in project, and begin configuring

---
### Concepts
- IMAP through c++
	- In order to receive email you must use IMAP or POP3, most modern email providers provide both, however IMAP is more supported, and better for multiple email clients as the email stays in the server so can be pulled by multiple clients
	- I was able to still use libcurl in order to do this, adding on to my email client class that I used for SMTP email sending
	- ```c++
	  bool fetchMail() {
        CURL *curl = curl_easy_init();
        if (!curl) {
            return false;
        }

        // Set up credentials
        curl_easy_setopt(curl, CURLOPT_USERNAME, senderUname_.c_str());
        curl_easy_setopt(curl, CURLOPT_PASSWORD, senderPswd_.c_str());
        std::string full = "imaps://" + std::string(imapServer_) + "/INBOX/;MAILINDEX=63";
        curl_easy_setopt(curl, CURLOPT_URL, full.c_str());

        // curl_easy_setopt(curl, CURLOPT_VERBOSE, 1L);


        // Force SSL/TLS
        curl_easy_setopt(curl, CURLOPT_USE_SSL, (long)CURLUSESSL_ALL);

        //perform
        CURLcode res = curl_easy_perform(curl);
        if(res != CURLE_OK) fprintf(stderr, "curl_easy_perform() failed: %s\n", curl_easy_strerror(res));

        //cleanup
        curl_easy_cleanup(curl);

        return res == CURLE_OK;
    }
	  ```
- Python packages and pip
	- By completing module one for PCAP I know more about how python libraries are sourced, made and stored
	- Especially using PyPi, or the cheese shop, which uses pip to install packages
- Linux
	- I relearned what the awk command does as I don't ever use it so it was a good refresher
	- It was especially interesting to learn that awk is a full fledged language in of itself
- HUNCH
	- I learned more about networking in hunch to configure vlan in my simulation
---
### Challenges
- Formatting email messages with IMAP(Not yet solved)
	- While I got IMAP to work using libcurl, many emails are sent using MIME format, which allows formany complexities beyond just text, including html, embedded links and much more that you see in emails every day
	- In order to solve this I plan to use the vmime library in conjunction with libcurl in order to create and process mime emails, and then send and recieve them using libcurl
- I didn't know some linux commands, in particularly awk
	- I relearned by paying attention to lesson and using awk --help to read the syntax and how it works simply
---
### Errors
- When creating the IMAP interface, I didn't configure the port correctly so the interface wouldn't connect to IMAP server
	- Fixed quickly by configureing port
- Setting up pfsense in GNS3
	- It took a lot of attempts to install pfsense, and then configure it for vlan, and I am still not done with it
---
### Action plan for next week
- Finish hunch plan for pdr, and print papers, laminate and get velcro for to put on poster
- Configure vmime library to format complex emails, especially to send as rendering html in terminal is very hard
	- Begin to learn qt library to create gui for email client if I get vmime working
