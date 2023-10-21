So there I was, sitting in the office, when a teammate came up to me. She told me she'd bought a new HDMI adapter, but something was off. Rather than functioning like a typical HDMI, it was detected as a USB storage device and prompted her to install some sort of .exe file for a driver.

What piqued my interest was the price. She bought this adapter from Amazon for just $16. Usually, that's the cost of a good, reliable piece of hardware—not the cheap ones that don't even work! Intrigued, I asked if I could borrow it to investigate further. While I'm not a malware analysis specialist, I am a hacker at heart, and I couldn't resist the urge to delve deeper into this mystery.

To start, I did a bit of research to see if anyone else had encountered HDMI adapters that behaved like USB drives. There weren't many results, but one article caught my eye. 
[Did I just infect my Win-10 computer with malware? - virus infection cybersecurity | Ask MetaFilter](https://ask.metafilter.com/367356/Did-I-just-infect-my-Win-10-computer-with-malware)

It talked about the possibility of the HDMI being backdoored, and the kicker? It was the exact same HDMI adapter we had. I also compared the product reviews, and they were consistent with what the article mentioned.

Next, I plugged the HDMI into my Windows testing machine, and—nothing. No detection, no functionality; it was as if the piece was broken. But then, I had an idea. Maybe the adapter needed a more 'common' setup to show its true colors. So, I grabbed another computer and plugged in the HDMI. Voila! It was detected as a USB drive. 

![[./images/Pasted image 20231021173939.png]]

The content of the drive had files for multiple operating systems: Windows, Mac, and Android. Additionally, there were two PDF readme files—one in English and another in Chinese. That last bit was particularly intriguing.

![[Pasted image 20231021174110.png]]

Why include a Chinese-language readme? Was this device provided by a Chinese company? Time to do some OSINT.
My first clue was the sticker on the device's packaging. It had details about the vendor:

![[images/2023-10-21 17_44_49-5B79E407-E780-4708-8C5B-100FA319221B.jpg ‎- Photos.png]]
Company Name: Zhou's Jade Star UG
Address: Brunnenallee 11A, 14478 Potsdam, Germany
Phone: +49 179 7962788
Email: Jadestar.eu@gmail.com

The first oddity that stood out was the unprofessional email address. A Gmail account for a business? That doesn't exactly scream 'reliable company,' does it? You'd think any reputable business would invest in a more professional email domain to uphold its image.

As for the address, it's listed as Potsdam, Germany—not China. However, this could merely be the distributor's address rather than that of the actual manufacturer. Upon searching for Zhou's Jade Star UG, I found that they had multiple products listed across various platforms, including eBay and Amazon.

![[images/2023-10-21 17_53_45-Zhou's Jade Star UG – Recherche Google et 14 pages de plus - Profil 1 – Microsof.png]]
At this point, I found myself at a crossroads: who really produced this device? To get to the bottom of this, I turned my attention to the mysterious .exe file that was part of the HDMI's 'features.' Oh, and another thing—I noticed that when the HDMI was plugged in, my computer's sound output was hijacked. Even though my headphones were set as the default audio device, the HDMI took precedence.

After observing the unusual behavior of this HDMI adapter, I felt it was crucial to dig deeper into the included .exe file named "ms display multidev v1.0.0.18.0." First, I opened the readme file, and to my astonishment, it explicitly instructed users to disable or uninstall antivirus software. That's quite a red flag, isn't it?

![[images/2023-10-21 03_39_08-readme.pdf et 11 pages de plus - Profil 1 – Microsoft​ Edge.png]]
Curious, I Googled the .exe file's name, and what I found was even more intriguing.
Not only does the program have a GitHub repository, but it also drew suspicions on Reddit, where someone questioned whether it was a form of malware. 

![[images/Pasted image 20231021182647.png]]

This Reddit user had even gone the extra mile, scanning the file on two platforms: VirusTotal and Hybrid-Analysis.

![[images/Pasted image 20231021185519.png]]

The results were inconsistent. While VirusTotal reported the file as clean, Hybrid-Analysis flagged it as malicious.

![[images/Pasted image 20231021185648.png]]
![[images/Pasted image 20231021185733.png]]

Returning to the GitHub repository, I noticed that all the descriptions were in Chinese. Stranger still was the "Issues" section, populated with unanswered questions about the suspicious nature of this .exe file. Most perplexingly, the repository hosted the executable without any source code. Numerous issues were open, querying the legitimacy of this software, yet none had been addressed to this day.

![[images/Pasted image 20231021185901.png]]
![[images/Pasted image 20231021185925.png]]
![[images/Pasted image 20231021185943.png]]

Motivated to uncover more, I decided to rescan the questionable .exe file on VirusTotal. This time, two antivirus vendors flagged it. While false positives are a possibility, especially from lesser-known vendors, what was more concerning were the relations tab results. The file had contacted several flagged IP addresses, and its "Execution Parents" field raised further red flags. One such IP, 13.107.4.50, had significant associations with various types of malware.

![[images/Pasted image 20231021192717.png]]
![[images/Pasted image 20231021192730.png]]

I also ran a scan of the .exe file using the AnyRun project, and the findings were unsettling.
The installation process revealed the publisher's name as "Ultrasemi Technology Development," which had no apparent connection with the German company we stumbled upon earlier. 

![[images/Pasted image 20231021192823.png]]
Moreover, the software attempted to load an unsigned driver a significant security concern.

![[images/Pasted image 20231021192858.png]]

AnyRun rated the software as malicious, highlighting its use of numerous unknown DLL files. All of this for an HDMI device? Seems unlikely.

![[images/Pasted image 20231021192959.png]]

My next step was to look into this new player, Ultrasemi Technology Development. A Google search led me to their website.

![[images/Pasted image 20231021193113.png]]
![[images/Pasted image 20231021193144.png]]

The site was in Chinese, which made sense of the Chinese readme file. Their GitHub repository was distinct from the one where the suspicious .exe file was uploaded and appeared abandoned.

![[images/Pasted image 20231021193215.png]]

The products they were selling primarily revolved around drive hardware. Intriguingly.
![[images/Pasted image 20231021193251.png]]
they also had a section dedicated to USB displays, showing how to install a program named "USMDisplay_windows.exe"—a different name than our initial "MSDisplay_MultiDev_v1.0.0.18.0.exe." Could it be a new version? That's yet to be determined
![[images/Pasted image 20231021193328.png]]

###Conclusion
While I can't definitively say whether this peculiar HDMI device is connected to malicious actors, its behavior is anything but ordinary. The journey through this labyrinth of oddities leaves us with two likely scenarios: either we're dealing with a backdoored device or the handiwork of inept developers who have resorted to unconventional coding to make a subpar gadget compatible with modern operating systems. In either case, my advice is simple: steer clear of such hardware.
