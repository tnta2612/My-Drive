Okay, I hope you all feel well today. From my side, I have 3 topics to discuss with you guys today. Admir, do you have anything you'd like to share or discuss today? Do you wanna start first or should I?

--------

Yeah thank you Admir for the sharing on your progress. And I'm sorry that I haven't had a chance to look at it in detail to give you more feedback. But I think the document looks much better now than last time. Because now I see that you also provide the evidence for each of the section you describe using testssl. The whole looks now more persuasive to me.

-----------------------------------------

First of all, I wanna tell you that finally I've finished working on the standard that is assigned to me. The one in German. Sorry the delay but it was quite a hard time to translate it into English, because I'm kind of familiar with the English terms. So the German terms in the standard were kind of weird to me. But finally, I did it. So now, I'd like to show you what I have. First of all, as I already told you, Lucas and Timon, the standard that I worked on is not a standalone standard. It is mostly built upon the 62443 series of standards, particularly the 62443 Part 4-2. Those are standards for IACS systems and IACS components in general. So as I said, without the 62443 standards, we cannot do anything further because almost everything in the medical standard is referenced to those 62443 standards.

But fortunately, surprise, I still have the complete 62443 series in my laptop because I used to work on them for my Bachelor thesis at Fraunhofer AISEC. So we were lucky. The 62443 series consists of about ten parts, from 1.1 upto 4.2. So I had everything I need to work on the medical standard. 

So the outcome, is this Excel file which documents all the security requirements that a medical device or a medical component should meet to achieve some kind of security level from 1 to 4. As you can see in this Excel file, the first column is the Index, from 1 to 96. So we have like 96 requirements in total. 

The second column contains the names of the requirements. So for example. Requirement 1.1 - Human user identification and authentication. Then, below it, we have 1.1 Requirement Enhancement 1 - Unique identification and authentication. And then 1.1 Requirement Enhancement 2 - Multifactor authentication for all interfaces. And so on. 

In next column, we have the requirements description, well what the requirements are actually about. I tried to summarize the description to make it compact and fit into the excel file. For the full description, one should refer to the standard itself.

In next 4 columns, I have the security levels from 1 to 4. And you can use them to find out all the security requirements that you need to fulfil to achieve a desired security level of 1, or 2, or 3, or 4. So for example, you can sort the column for security level 1, then you have a list of requirements that you need to meet to achieve security level 1, up to here. And you can do the same for the other ones.

The last column is about how this requirement is met for a specific medical component. The column is still empty because we need to fill it out for each individual medical device or medical software that we have. Which means, for each medical component, we need such a separate Excel file to evaluate which requirements are met. And we do it for each medical component. I'm not sure if it makes sense for me to do it on my own because I'm not the one that knows the product best. So I guess it might be the product owner or Lucas who can easily fill out the last column for each component.

So to conclude, regarding the security requirements and security risks, we're more or less finished. All the security requirements are there, and we can use it in the future to evaluate our products.
Regarding the security risks, there's one thing Admir and I still need to do. We need to merge the risks found by Admir and the risks found be me into one document, and then we might also need to document how the risks are addressed. I don't know if you already did it, Admir? Because last time you asked for my documents on the risks and requirements. I don't know if you already merge them together?

If you finish merging them, then you can also document how the risks are mitigated, or what countermeasures are in place. You can use your own knowledge about the system to do that, or you can ask and discuss with Lucas. Do wanna do it Admir? Or should I do it? It's actually not a problem for me, but I can only do it latter because now I'm kind of busy with the static analysis of the whole code base, which is my second topic for today. Do you guys have any questions regarding the security requirements? Otherwise, I'd like to go on with my second topic for today, which is about: how attackers get access to the backend servers through the Git repositories.

--------------------------------------------

Okay to begin with, you know, when writing code, developers tend to store the credentials somewhere in their code. For example, you have a web application which uses an API key or token to access the backend database. So you need to give your web application this API key or token so that it can use the token to access the database. So the question is, how should you give the key to it? One way is to put the token right in the code, something like this. In that case, we say that the credentials are hard-coded in the code. But obviously, that's not a secure way to do it, because anyone having access to the Git repository can see the credentials and use it to connect to the backend servers and do anything they want.

When I was working at the Stadtwerke München, just as a working student, I had access to many Git repositories of the company. And in most of them, I could find some kind of credentials which I could use to access the backend servers to retrieve sensitive information or even change the configuration of the backend servers. All of those were possible, just because the credentials were hard-coded and pushed onto the Git repositories, where anyone can see it. That is a big security issue. Therefore I decided to go through our whole codebase to see if I'm able to find some sensitive credentials. And indeed, I found some, which I'd like to show you today.

So first, I scanned through the eversion-fastapi repository. I found the Firestore Credentials which contains the private key, as you can see here. Well, the private key is exposed and anyone knows it and can use it to connect to the Firestore instance, and do whatever they want. I also found the credentials for the InfluxDB which contains a token to access the database server. And as confirmed by Lucas, the token is valid permanently and doesn't expire. So when it is exposed, anyone can use it to access the database for quite a long time.

Then, in a different file, I also found the token used by the InfluxDBClient to connect to the InfluxDB. I found many other token and private keys in other files, or in other commits. I included all the references to the secrets that I found in this document. I will send it to you Lucas, so that you can have a look and check if you have remove all of them from the Git repositories completely.

I already told Lucas about this issue yesterday and Lucas said that he removed them from the git repository. It's good to hear that, but I think you might need to check it again with this comprehensive list to make sure that you don't miss anything. One thing I want to mention is that, it's good that you remove the credentials from the Git repository, but you also need to make sure that you also remove them from the commit history. Yeah because, obviously, they still exist in the history even after you remove them from the repository. So make sure to also remove them from the commit history.

If you ask me how you should remove the credentials from the repository history, well I don't know it by heart because I'm not a developer. But I included the references there where you can have a look on how to do it. And instead of hard-coding the secrets, we can use Github Actions secrets or Codespaces secrets, well again, just for your reference on how we can do it in a different way.

And I did the same for the all the Git repositories that we have. I scanned through the whole code space. For some repositories, fortunately, there's no leaks found. All the leaks that I found are documented in this document. You can have a look at them and try to remove them from the Git repositories.

--------------------------------

Okay that's it for my second topic for today. Any questions? Otherwise I'd like to go on with the last topic: the web application. Actually, there's nothing I want to discuss or show you. I just have some questions. 

This is the web application, the dashboard. I don't know if you already had a look at it Admir? When I click on Meine Messungen, I got this. It seems to be loading permanently. Lucas, I wanna ask if this is what I'm supposed to see? Or is there any problem with it, because it seems to be loading permanently?

And I think there's still no data there, right? I wanna ask how I can get some sample data to do some testing? Because without data, I'm quite not sure how I should proceed.

---------

And one more thing, I see this form on the website where we can register for news from the company. So my questions is: after filling out this form and click Absenden, all the data will be save in a database, right? The customer will then receive a confirmation email. And the customer needs to click on the verification link to confirm their email address. What if the customer doesn't click on that verification link?

Is it possible to fill out this form and send it multiple times using just one email address, and receive the confirmation mail multiple times to that email address? I kind of need it for testing purposes. I tried that yesterday. But it seemed like it sends only one confirmation mail to an email address, regardless of the customer clicking on the verification link or not. I couldn't get it to send me a second confirmation mail. Do you know how I can do it? Or maybe can you explain in detail how this works in the backend? Maybe I can think of a way to exploit it. 

----------------------------

Yeah I think that's it for today from my side. This week I will test the web application, the authentication service and the database. 

-----------------------------

So my last question: where should I upload my documents to share with you guys? On OneDrive?Okay, I hope you all feel well today. From my side, I have 3 topics to discuss with you guys today. Admir, do you have anything you'd like to share or discuss today? Do you wanna start first or should I?

--------

Yeah thank you Admir for the sharing on your progress. And I'm sorry that I haven't had a chance to look at it in detail to give you more feedback. But I think the document looks much better now than last time. Because now I see that you also provide the evidence for each of the section you describe using testssl. The whole looks now more persuasive to me.

-----------------------------------------

First of all, I wanna tell you that finally I've finished working on the standard that is assigned to me. The one in German. Sorry the delay but it was quite a hard time to translate it into English, because I'm kind of familiar with the English terms. So the German terms in the standard were kind of weird to me. But finally, I did it. So now, I'd like to show you what I have. First of all, as I already told you, Lucas and Timon, the standard that I worked on is not a standalone standard. It is mostly built upon the 62443 series of standards, particularly the 62443 Part 4-2. Those are standards for IACS systems and IACS components in general. So as I said, without the 62443 standards, we cannot do anything further because almost everything in the medical standard is referenced to those 62443 standards.

But fortunately, surprise, I still have the complete 62443 series in my laptop because I used to work on them for my Bachelor thesis at Fraunhofer AISEC. So we were lucky. The 62443 series consists of about ten parts, from 1.1 upto 4.2. So I had everything I need to work on the medical standard. 

So the outcome, is this Excel file which documents all the security requirements that a medical device or a medical component should meet to achieve some kind of security level from 1 to 4. As you can see in this Excel file, the first column is the Index, from 1 to 96. So we have like 96 requirements in total. 

The second column contains the names of the requirements. So for example. Requirement 1.1 - Human user identification and authentication. Then, below it, we have 1.1 Requirement Enhancement 1 - Unique identification and authentication. And then 1.1 Requirement Enhancement 2 - Multifactor authentication for all interfaces. And so on. 

In next column, we have the requirements description, well what the requirements are actually about. I tried to summarize the description to make it compact and fit into the excel file. For the full description, one should refer to the standard itself.

In next 4 columns, I have the security levels from 1 to 4. And you can use them to find out all the security requirements that you need to fulfil to achieve a desired security level of 1, or 2, or 3, or 4. So for example, you can sort the column for security level 1, then you have a list of requirements that you need to meet to achieve security level 1, up to here. And you can do the same for the other ones.

The last column is about how this requirement is met for a specific medical component. The column is still empty because we need to fill it out for each individual medical device or medical software that we have. Which means, for each medical component, we need such a separate Excel file to evaluate which requirements are met. And we do it for each medical component. I'm not sure if it makes sense for me to do it on my own because I'm not the one that knows the product best. So I guess it might be the product owner or Lucas who can easily fill out the last column for each component.

So to conclude, regarding the security requirements and security risks, we're more or less finished. All the security requirements are there, and we can use it in the future to evaluate our products.
Regarding the security risks, there's one thing Admir and I still need to do. We need to merge the risks found by Admir and the risks found be me into one document, and then we might also need to document how the risks are addressed. I don't know if you already did it, Admir? Because last time you asked for my documents on the risks and requirements. I don't know if you already merge them together?

If you finish merging them, then you can also document how the risks are mitigated, or what countermeasures are in place. You can use your own knowledge about the system to do that, or you can ask and discuss with Lucas. Do wanna do it Admir? Or should I do it? It's actually not a problem for me, but I can only do it latter because now I'm kind of busy with the static analysis of the whole code base, which is my second topic for today. Do you guys have any questions regarding the security requirements? Otherwise, I'd like to go on with my second topic for today, which is about: how attackers get access to the backend servers through the Git repositories.

--------------------------------------------

Okay to begin with, you know, when writing code, developers tend to store the credentials somewhere in their code. For example, you have a web application which uses an API key or token to access the backend database. So you need to give your web application this API key or token so that it can use the token to access the database. So the question is, how should you give the key to it? One way is to put the token right in the code, something like this. In that case, we say that the credentials are hard-coded in the code. But obviously, that's not a secure way to do it, because anyone having access to the Git repository can see the credentials and use it to connect to the backend servers and do anything they want.

When I was working at the Stadtwerke München, just as a working student, I had access to many Git repositories of the company. And in most of them, I could find some kind of credentials which I could use to access the backend servers to retrieve sensitive information or even change the configuration of the backend servers. All of those were possible, just because the credentials were hard-coded and pushed onto the Git repositories, where anyone can see it. That is a big security issue. Therefore I decided to go through our whole codebase to see if I'm able to find some sensitive credentials. And indeed, I found some, which I'd like to show you today.

So first, I scanned through the eversion-fastapi repository. I found the Firestore Credentials which contains the private key, as you can see here. Well, the private key is exposed and anyone knows it and can use it to connect to the Firestore instance, and do whatever they want. I also found the credentials for the InfluxDB which contains a token to access the database server. And as confirmed by Lucas, the token is valid permanently and doesn't expire. So when it is exposed, anyone can use it to access the database for quite a long time.

Then, in a different file, I also found the token used by the InfluxDBClient to connect to the InfluxDB. I found many other token and private keys in other files, or in other commits. I included all the references to the secrets that I found in this document. I will send it to you Lucas, so that you can have a look and check if you have remove all of them from the Git repositories completely.

I already told Lucas about this issue yesterday and Lucas said that he removed them from the git repository. It's good to hear that, but I think you might need to check it again with this comprehensive list to make sure that you don't miss anything. One thing I want to mention is that, it's good that you remove the credentials from the Git repository, but you also need to make sure that you also remove them from the commit history. Yeah because, obviously, they still exist in the history even after you remove them from the repository. So make sure to also remove them from the commit history.

If you ask me how you should remove the credentials from the repository history, well I don't know it by heart because I'm not a developer. But I included the references there where you can have a look on how to do it. And instead of hard-coding the secrets, we can use Github Actions secrets or Codespaces secrets, well again, just for your reference on how we can do it in a different way.

And I did the same for the all the Git repositories that we have. I scanned through the whole code space. For some repositories, fortunately, there's no leaks found. All the leaks that I found are documented in this document. You can have a look at them and try to remove them from the Git repositories.

--------------------------------

Okay that's it for my second topic for today. Any questions? Otherwise I'd like to go on with the last topic: the web application. Actually, there's nothing I want to discuss or show you. I just have some questions. 

This is the web application, the dashboard. I don't know if you already had a look at it Admir? When I click on Meine Messungen, I got this. It seems to be loading permanently. Lucas, I wanna ask if this is what I'm supposed to see? Or is there any problem with it, because it seems to be loading permanently?

And I think there's still no data there, right? I wanna ask how I can get some sample data to do some testing? Because without data, I'm quite not sure how I should proceed.

---------

And one more thing, I see this form on the website where we can register for news from the company. So my questions is: after filling out this form and click Absenden, all the data will be save in a database, right? The customer will then receive a confirmation email. And the customer needs to click on the verification link to confirm their email address. What if the customer doesn't click on that verification link?

Is it possible to fill out this form and send it multiple times using just one email address, and receive the confirmation mail multiple times to that email address? I kind of need it for testing purposes. I tried that yesterday. But it seemed like it sends only one confirmation mail to an email address, regardless of the customer clicking on the verification link or not. I couldn't get it to send me a second confirmation mail. Do you know how I can do it? Or maybe can you explain in detail how this works in the backend? Maybe I can think of a way to exploit it. 

----------------------------

Yeah I think that's it for today from my side. This week I will test the web application, the authentication service and the database. 

-----------------------------

So my last question: where should I upload my documents to share with you guys? On OneDrive?