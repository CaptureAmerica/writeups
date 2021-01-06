---
title: "Passed OSCP Exam (I Tried Harder)"
date: 2021-01-05T150:00:00+09:00
lastmod: 2021-01-05T15:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["OSCP"]
categories: ["OSCP"]
author: ""
---
{{% right %}}
<a href="https://captureamerica.github.io/writeups/post/tried_harder_oscp/">
<img src="https://captureamerica.github.io/writeups/img/Jp.png" alt="English">
</a>
{{% /right %}}

I passed the [OSCP](https://www.offensive-security.com/) (Offensive Security Certified Professional) Exam at the first attempt.

I would like to thank my family, my manager and company I work with for the support.

<br />

In this blog, I'm going to write something useful (I think) for OSCP exam takers.


<br /><br />
## My PC setup

- **Macbook Air (16GB RAM)**
<br />
Previously, I was mainly using Kali on Windows 8 with VirtualBox. I thought the Windows 8 machine wasn't powerful enough for the OSCP exam, so I decided to buy a new PC. I bought a Macbook Air (16GB RAM).
<br /><br />
Since then, I've been using Mac as a main PC and Kali as a sub PC. I launch OpenVPN & Burp Suite on Kali (sometimes I also run Wireshark for troubleshooting), then I ssh into Kali from Mac PC using [iTerm2](https://iterm2.com/), or FireFox pointing to Kali as a proxy.


- **VirtualBox**
<br />
VMWare Fusion is recommended by Offsec. Just in September, [VMWare Fusion 12 Free version](https://my.vmware.com/web/vmware/evalcenter?p=fusion-player-personal) was released, so I tried it. But I decided to use VirtualBox since I had one Kali image I've been using for CTF for some time. I didn't have any trouble with VirtualBox for the OSCP lab and the exam.
<br /><br />
By the way, the username on Kali is "captureamerica". I used the user for the OSCP lab report and the exam. (So, the name appears in the report).

- **Emacs**
<br />
I've been using Emacs editor for a long time. Here I also used Emacs for taking notes and for the OSCP reports. The output of [Powerless.bat](https://github.com/M4ximuss/Powerless/blob/master/Powerless.bat) has no color, so I used Emacs to highlight some keywords. I'll write the details below.

- **LibreOffice**
<br />
For my own cheatsheet, I used LibreOffice.
<br />

- **Two External Monitors**

- **Standing Desk**
<br />
During the OSCP exam, I kept standing up for almost 21 hours.


<br /><br />
## TIPS

- **Avoid software version up**
<br />
I suggest not to update any software or host OS as much as possible until the exam is over. Actually, I once updated VirtualBox, then I faced an issue where Kali was unable to boot up. Luckily, I could fix it soon (it was something related to the audio setting). MacOS Big Sur was released just before my exam, but I just ignored it.

- **RDP**
<br />
When accessing lab machines via RDP, I used SSH Tunnel through Kali as follows so that I could use Microsoft Remote Desktop on Mac. File copy is much easier by doing this.
<br /><br />
`$ ssh captureamerica@kali -L 13389:IPADDR:3389`
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_RDP.png" alt="OSCP_RDP.png">

- **Highlighting text with Emacs**
<br />
I googled and anyhow I defined some regex in .emacs as follows. Initially, I added some definitions for [Powerless.bat](https://github.com/M4ximuss/Powerless/blob/master/Powerless.bat), then later on I added some for the output of gobuster, nmap, etc.
<br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_Emacs.png" alt="OSCP_Emacs.png">
<br /><br />
Opening [Powerless.bat](https://github.com/M4ximuss/Powerless/blob/master/Powerless.bat) output with Emacs: (btw, I modified Powerless.bat itself to log commands that it runs.)
<br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_Emacs_Powerless.png" alt="OSCP_Emacs_Powerless.png">
<br />
(This output was taken on [Metasploitable3](https://github.com/rapid7/metasploitable3), not OSCP machine)
<br /><br />
Opening nmap output with Emacs:
<br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_Emacs_nmap.png" alt="OSCP_Emacs_nmap.png">
<br />
(This output was taken on [Metasploitable3](https://github.com/rapid7/metasploitable3), not OSCP machine)
<br /><br />
Without color, it's not very easy to find key parts and in fact I have missed some during enumerations, so I think this is important.

- **Add the self IP to the IME dictionary**
<br />
For some commands, the self IP address is often used, so I added it to the Japanese Input dictionary to save some time.
<br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_IME.png" alt="OSCP_IME.png">
<br />
(This is not the IP address used for OSCP)

- **Copy & Paste commands**
<br />
I collected all the commands I often use in one text file. When I started working on a new machine, I used the text file and replaced "IPADDR" "MYADDR" using Emacs or sed command, then most of the time, I just copy & paste those commands.
<br />
This is useful to prevent typo and to save time.
<br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_ToDo.png" alt="OSCP_ToDo.png">
<br /><br />
(One negative part is, it's difficult to memorize commands if relying on this too much.)



<!--
- **ツールのカスタマイズ**
<br />
[Powerless.bat](https://github.com/M4ximuss/Powerless/blob/master/Powerless.bat)とLinEnum.shは、自分で実行したいコマンドを追加したり、コマンドの順番を入れ替えたり、自分用に使いやすいようにカスタマイズしました。
<br /><br />
あと、WebアプリでLFI、SQL Injection、Command Injectionを見つけるツールを自作して使ったりもしました。あんまり活躍しなかったけど。。
-->



<br /><br />
## Writing Reports

I used this Mark Down type of report.
<br />
https://github.com/noraj/OSCP-Exam-Report-Template-Markdown

### Pros

- Reports can be written by using Emacs.

- Light and smooth (I haven't tried writing a report that has more than 300 pages using LibreOffice, but I guess it becomes sluggish.)

### Cons

- After converting to PDF, the contents sometimes look different (This might be the same when using Office software.)

- The coloring of the code part isn't nice (I fixed this using an application. See below.)


### Tools I used

- [typora](https://typora.io/)
<br />
I edited the .md file using Emacs and checked the design on Typora simultaneously. Though the PDF content looked different from the display of Typora, it's still useful.

- [PDF Element](https://pdf.wondershare.co.jp/)
<br />
I bought this application to fix the color of the code part.
<br />
Firstly, I used "HTML" for the code part (for instance, even if the code is "Python", I specify "HTML") so that it contains less color. After converting it to PDF, I changed the color using this tool.
<br />
Not only colors, but also I added/deleted some texts using PDF Element.
<br />
I also tried other PDF tools (e.g. Preview.app on Mac, Adobe Acrobat DC, Google Doc Plugin, etc), but as far as I tested, PDF Element was the best among them for this purpose.
<br /><br />
It's just like this,
<br />
When setting "bash" for the code: (not so nice, is it?)
<br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_bash.png" alt="OSCP_bash.png">
<br /><br />
After the modification by using PDF Element:
<br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_HTML_PDFElement.png" alt="OSCP_HTML_PDFElement.png">
<br />
(Again, this output was taken on [Metasploitable3](https://github.com/rapid7/metasploitable3), not OSCP machine)



<br /><br />
## Lab Report

A lot of time is required for the Exercise. So, skipping the Lab Report is one of the options, but in my opinion it's good to spend some time to complete the lab report.
<br /><br />
The reasons are, 

- **5 points are added to the exam points**
<br />
It's just 5 points but I think it's important how soon we can reach 70 points during the exam. The more we take time, the more we feel nervous.

- **Good practice for the exam report**
<br />
You may be very tired on the 2nd day and the time is limited. (Actually, I was very exhausted on the 2nd day.) It's good to get used to writing reports.

- **Exercise itself is meaningful**
<br />
I sometimes referred to the Exercise report I wrote during the course, like, "ah, I remember I did the same during the exercise!" kind of thing.

- **CPE 40 points are given**
<br />
I just found this out after the exam.
<br />
FYI. Here's how to submit CPE.
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_CPE1.png" alt="OSCP_CPE1.png">
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_CPE2.png" alt="OSCP_CPE2.png">
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/OSCP_CPE3.png" alt="OSCP_CPE3.png">



<br /><br />
## The skillset I had before taking the OSCP course.

- **[GPEN](https://www.giac.org/certification/penetration-tester-gpen)**
<br />
Got this 3 years ago. It was about to expire in Dec 2020. That's one of the reasons I wanted to get OSCP. Sometimes, I referred to the notes I took during SANS training.

- **[CISSP](https://www.isc2.org/)**
<br />
Got this about 6 years ago. In 2020, I did [HTB](https://www.hackthebox.eu/) to earn CPEs. I thought the 6 hours exam of CISSP was quite long but it's much shorter compared to the OSCP exam.

- **[LPIC-2](https://www.lpi.org/our-certifications/lpic-2-overview)**
<br />
I got Level 1 & 2.

- **Pentest**
<br />
I'm working in a cyber security related IT company but my job is not related to Pentest. I used to play [HTB](https://www.hackthebox.eu/) but I stopped about 6 months ago after I earned enough CPEs to maintain CISSP.

- **Programming**
<br />
I was working as a developer a long time ago and at that time I was using C language.
<br />
I've been playing CTF for one and a half years and learned some Python.

- **English**
<br />
I'm Japanese but I've been living in an English-speaking country for about 15 years. OSCP manual, video, forum, and most of the information for OSCP found on the internet are all in English, so it's good to have English skills.

- **Windows**
<br />
I know how to use Windows, but I wasn't very familiar with the commands, PowerShell, Active Directory. I had a little knowledge at the beginning of the OSCP course.


<br /><br />
## How I spent 3 months

This is just FYI. The way I did might not be the best.

- **The first month**
<br />
I began with the lab. I just tried to get as many as proof.txt. I planned to review all the machines later on anyway.
<br />
When I was stuck on a machine, I went to the forum right away to find the answer. Within a month, I finished Public (except for a few) and DEV network. I simply enjoyed the first month.
<br />
I also kept playing the video over and over while I was working on the lab machines.

- **The second month**
<br />
I finished all the exercises and lab report for 13 machines within 3 weeks. Then I reviewed all the Public machines and DEV network. I read some books (One is all about Mimikatz. Another one is "The Hacker Playbook 3 (Author: Peter Kim)". Both are written in Japanese), and I watched [Udemy](https://www.udemy.com/) ("Practical Ethical Hacking - The Complete Course").
<br />
I felt tired doing exercises but I tried harder.

- **The third month**
<br />
I did the rest of the lab machines within 10 days. Lab was completed.
<br />
For the rest of the days, I worked on [Metasploitable3](https://github.com/rapid7/metasploitable3), HTB & VulnHub machines in this [list](https://docs.google.com/spreadsheets/d/1dwSMIAPIam0PuRBkCiDI88pU3yzrqqHkDtBngUHNCw8/edit#gid=0). Honestly, I lost confidence since I could not solve those HTB & VulnHub machines without looking at the walkthrough.
<br />
OSCP exam was on Monday. I looked through the walkthrough documents of all the HTB & Vulnhub machines (in the list above) that I could not try. I didn't have enough time, so I had no choice.
<br />
On Sunday, I did one [BOF](https://github.com/freddiebarrsmith/Buffer-Overflow-Exploit-Development-Practice).
<br />
<br />
Before the exam, I could not sleep well, maybe because I was too nervous.


<br /><br />
## Exam

- **Proctor setup (45 min)**
<br />
It was supposed to finish in 15 min but it took 45 min.
<br /><br />
If you live in a different country from the passport issued country, another ID is necessary. So I suggest preparing it beforehand.

- **BOF (50 min、25 points)**
<br />
Most people begin with BOF. I just followed.

- **Machine A (45 min、20 points)**
<br />

- **Machine B (3 hours、10 points)**
<br />
Reluctantly, I had to use Metasploit..

- **Machine C (7 hours、20 points)**
<br />

- **Machine D (6 hours、25 points)**
<br />
<br />
I spent 21 hours (including the break time and the final review time) to complete the exam.
<br />
**I really tried harder.**
<br /><br />
Here's the timeline. The exam began at 7:00, then I reached 70 points (this includes the 5 points of the lab report) at 18:00. After I reached 70, I asked the proctor, "Am I allowed to play music?", then I got "Yes, you may", so started playing Spotify. I sang songs and worked on the machines as usual.
<br /><br />
I ended the exam at 4:00.

- **Exam Report**
<br />
I took a nap and started working on the report. I spent roughly 4 hours. It contained about 40 pages.

- **Result**
<br />
I got the result on the next day. I was glad to get the result so quickly because I was nervous after I sent the Exam reports.
<br />


<br /><br />
## Summary

Throughout the OSCP course, I learned lots of stuff and I enjoyed so much. It was a very meaningful precious time. I still have a lot of things I do not know, so I will continue to study harder.

I think the most important thing is to enjoy studying. Actually, I'm not planning to become a pentester. I intentionally try to enjoy it like doing CTF.

For the past 3 months, I stopped watching NetFlix, playing games, and I was very focused on OSCP, but I didn't feel much stress.

The OSCP manual, video, lab and exam are all well made and organized, I think it's worth paying for the course.

<br />

To: OSCP exam takers,

**Please enjoy and TRY HARDER!!**


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
