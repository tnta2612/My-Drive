
Yeah I think the Incident Response Plan now looks much better than last time, because now you categorise the possible incidents and act accordingly. It contains more details, which is great.

I just wanna know if you used any of the templates for Incident Response Plan that I sent you last week? I just wanna know if any of them is useful for creating an Incident Response Plan.

Okay, just one thing more I want to mention. I see some figures in your documents, well some images, I don't know if you created them yourself or get them from somewhere else. But, just don't forget to put the sources in your final report. Otherwise, it could be seen as plagiarism. Because that problem is really strict at the TUM. I don't know if you've ever written any report at the TUM, I guess you have already, right. Just in case if you haven't, it's just a kind reminder from my side, so that you're not gonna encounter any problem in the end.

---------------------------------------------------------------

Yeah last week I tried some kind of NoSQL injection on the MongoDB. But good news is that it didn't work. So the MongoDB might not be vulnerable to injection attacks for now. I also tested the FastAPI trying to find more vulnerability, but I also didn't find any. This week, if I have time, I will try more ways for NoSQL injection on the MongoDB, because I still have the feeling that it might be vulnerable somehow. Or I think I should start working on the Mobile App because I already received the test version of it from Lucas yesterday. 

So this week I don't have any new finding to show you. But I have one other thing. Have you guys ever heard of the CVSS Scoring system? Well, in short, it's a scoring framework to evaluate how severe is a vulnerability in a system, so that we will have concrete severity scores to determine which vulnerabilities are more important to fix promptly and which vulnerabilities are not so important and can wait to be fixed latter. 

Yeah last week, I tried to assign a CVSS score for every finding that we have, so that you Lucas can better prioritise them. Well, you can fix the most severe vulnerabilities first. And then if you want and have resources, you can fixed other vulnerabilities that are less severe.

Okay so now the question is: do you guys have interest in it? I can talk a bit about this CVSS Scoring system today. Otherwise, if you're not interested. I don't think I have any think to show you today.

---------------------------------------------------------------

So last weeks I showed you many findings, many vulnerabilities. And I told you that some of them are really severe and need to be fixed, because if they are exploited by attackers, we will have severe consequences. I also told you that some of them are just informational, which means you can fix them if you want, otherwise it should still be okay.

---------------------------------------------------------------

So I told you that some vulnerabilities are less severe, some are more severe, and some are extremely severe. But it was just an informal way to describe the severity of the vulnerabilities. So the question is: How can we formally represent the severity with concrete numbers to better evaluate and prioritise the vulnerabilities. One popular solution is this CVSS Scoring system, which is really well-known in the field of IT Security and is often used to evaluate and prioritise vulnerabilities in a system. 

---------------------------------------------------------------

So today I want to give you an overview of this CVSS scoring system, so that when you read my report on the findings, you can understand what these scores mean. I think understanding the CVSS scoring system is really beneficial for you Admir if you want to pursuit a career path in IT Security, because CVSS is really often used in security reports. And for you Lucas and Timon, I think it might also be beneficial for you in a way that you can understand what these CVSS scores means when you read security reports for your products. Therefore, today I want to give you an overview of it, not the details, but just an overview or the most important metrics so that you know what it is about and how to use it.

---------------------------------------------------------------

Okay so this is the CVSS calculator which helps us to determine the CVSS score for a vulnerability. We only need to choose one suitable option for each type of metrics below. So let's start with the Attack Vector. We have 4 choices: Network, Adjacent, Local, and Physical. 

---------------------------------------------------------------


Network means that the attack can be carried out over the Internet, yeah from a place far away. Adjacent means that the attack can only be carried out in an adjacent environment, such as over Bluetooth, over NFC, or if the attacker is in the local network of the company. Local means that attackers need to have access to the target locally, like having access to the keyboard or having access via Secure Shell, well SSH. And Physical means that attackers need to have physical access to the target, such as access to the CPU or RAM to carry out an attack. So normally we will choose Network because most of the attacks we normally have should be carried out over the Internet.

---------------------------------------------------------------

Then we have Attack Complexity, it could be Low or High. High complexity means that attackers need to successfully carry out additional attacks before the real attack can be carried out successfully. Otherwise, the complexity should be low.

---------------------------------------------------------------

Then we have Attack Requirements, which means that whether an attacker needs to meet some initial requirements to be able to carry out an attack. Well for example, you guys know the Man-in-the-Middle attack, right? Well the requirement for that attack to be possible is that the attacker first needs to somehow place himself between the victims. Otherwise the attack would not be possible. Yeh, that's the Attack Requirements.

---------------------------------------------------------------

Then we have Privileges Required. High means that an attacker needs high privileges to carry out an attack, for example the attacker needs to be an administrator. Low means that the attacker needs low privileges to carry out the attack, for example the attacker needs to be a normal user in the system. None means that the attacker doesn't need any kind of privilege, he can just carry out the attack anonymously.

---------------------------------------------------------------

Okay the next one: User Interaction indicating if User Interaction is required or not to carry out an attack. When User Interaction is required, we have Passive or Active. An example for Passive is a XSS attack, where a user needs to browse a website to trigger the malicious payload on the website. Well the user doesn't know about that and the attack is carried out automatically. So it should be Passive. An example for Active is that, when a user has downloaded a malicious Excel file and tries to open it, a security warning pops up and says that this Excel file might contains malicious macro code which will be executed if the user proceed. The user accepts the security warning and open the file, and the malicious macro code is executed. So the user actively interacted with the malicious file in this scenario. So the User Interaction should be classified as Active.

---------------------------------------------------------------

Okay the next ones are much easier to explain: Confidentiality, Integrity, and Availability. So these just mean how each of these metrics is affected if the vulnerability is successfully exploited. It can be High, Low, or None. For example, an exposure of personal user data might have a high impact on the Confidentiality of the system. A DDoS attack might have a low or high impact on the Availability of the system. And so on.

---------------------------------------------------------------

And here below we have them once again: the Confidentiality, Integrity, and Availability. But this time not for the system itself but for the subsequent systems in place, well the systems that are related to the one being exploited. For example, if an attacker exploits the FastAPI to delete all measurement data, it will affect the Availability of the system, right?

But think about the mobile application or the web application, they also use those measurements data to display it to users. If those data in unavailable, it also affects the availability of the mobile application or the web application. Therefore, we have these metrics for subsequent system impact.

---------------------------------------------------------------

Well enough talking, let's see a concrete example in our case. But before that, any question so far?

Okay let's take this vulnerability as an example: Unauthenticated deletion of measurement data, well anyone on the Internet can delete all the measurement data in our database. So let's see what CVSS score it should have

---------------------------------------------------------------

Well, the Attack Vector is Network because the attack can be carried out over the Internet. The Attack Complexity is low because it's quite easy to carry out this attack. Attack Requirements are None, there's no initial requirement to carry out this attack. Privileges Required is None. The attacker doesn't need any account or any API token to carry out this attack. User Interaction is None because the attack doesn't require any user interaction to be successful. 

This attack is about deleting measurement data. So the impact on Confidentiality and Integrity is None. But the impact on Availability is High, because we lose all the data. For subsequent system, well for example the mobile app or the web app, the impact on Confidentiality and Integrity is None, but the impact on the Availability is also High. So in total we have a score of 9.2, which is classified as Critical and should be fixed as soon as possible.

Yeah that's it. Any questions?

---------------------------------------------------------------

There are many other metrics that we can use to evaluate a vulnerability. But I think the Base Metrics are often enough for this purpose. If you want to dive deeper into it, just read the specification document.



Yeah that's it from me for today.

---------------------------------------------------------------


