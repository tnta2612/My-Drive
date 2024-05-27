
Okay, Yeh I did some testing on the web application and found some potential issues, or vulnerabilities. I haven't finished with the testing yet but I do have found some vulnerabilities which I can show you guys today. I documented them in a file so that you guys can have a look at them latter on. But first before showing you the vulnerabilities, I want to make it clear that: although they are potential vulnerabilities, we don't have to necessarily mitigate all of them. Vulnerabilities can be mitigated, or accepted, depending on the specific situation and the capability of the company. You know, not all vulnerabilities can be addressed. We can give priority on the severe vulnerabilities, mitigate them and accept the vulnerabilities that are not so severe. Well I would say it totally depends on you Lucas to decide if the company should mitigate a vulnerability or not. If you decide not to mitigate it, you can always accept the vulnerability, given that you know and understand the risks associated with that vulnerability. 

-----------------------------

Okay enough warnings for the preface, let's go into the vulnerabilities. So first I checked the SSL/TLS connections with testssl to find potential vulnerabilities on the network level. Admir did that already and showed us the results last week, so I think I don't need to talk about that anymore. I just put it there for completeness. But overall, there're no vulnerability found on the SSL/TLS connections.

-----------------------------

Then, I used gobuster to scan for hidden files and hidden directories in the web application. Unfortunately, there's no hidden file or directory to be found. That is sad from an attacker's perspective. But that is good for the product owner and the web application itself.

I also scanned for subdomains of eversion.tech and you can see the results. Well they are not security issues themselves, but they are rather informational so that Lucas you can make sure that those subdomains are still active and maintained by the company. If not, they should be removed and updated.

-----------------------------

Okay, now let's move on to somewhat real security issues of the web application. So first of all, let's talk about the Content Security Policy, or CSP. So first the question: do you know what CSP is or should I explain?

Okay CSP is an HTTP header that is included from server responses, as you can see here in the screenshot. The CSP header contains the directives specifying which resources can be loaded and also from which domains they are allowed to be loaded. For example, we can specify from which domains the javascripts are allowed to be loaded, using the script-src directive. Or we can specify from which domains the images are allowed to be loaded, using the img-src directive, and so on. So with Content-Security-Policy, we can control which resources from which domains are allowed to be loaded into our web application. If we take advantage of the Content-Security-Policy properly, we can prevent certain types of attacks such as cross-site scripting, or clickjacking.

Okay, so now, what is the finding on our web application. The web application uses this CSP header: ```Content-Security-Policy: script-src 'none'; frame-src 'none'; sandbox;```

This CSP disallows all scripts to be executed. It also disallows all content to be framed. So overall, this is quite a secure CSP because it doesn't allow any script to be executed on the page. However, I still have 2 recommendations for this CSP. First, we should also set the object-src to none to prevent loading of plugin content such as Flash or Java applets because those could be exploited to execute code on the page. So it's better to also set the object-src to none.

Second, the CSP header is not consistent across all requests and responses. For example, for this request, there's no CSP header from the server. So there's no CSP protection for the homepage. And yeah, we should set the CSP header consistently in all responses from the server to make use of the CSP protection from XXS and clickjacking attacks.

-----------------------------

So talking about clickjacking attacks, I have the next finding for it. Indeed, because there's no protection in place, the web application is vulnerable to click-jacking attacks. Do you guys know what a click-jacking attack is or should I explain shortly?

Have you ever heard of someone clicking a button on a weird website saying "Click on the button below the get your free iPhone 15" and then after click on that button, he inadvertently transferred his bitcoins to an attacker's account? Yeh, that might be a click-jacking attack. By clicking on the button, the victim unknowingly confirmed the transaction which transferred his bitcoins to the attacker.

So the real website for transferring bitcoins is hidden behind some weird page saying that "click on this button and receive an iPhone". The victim thinks he clicks on the button and gets the present. But actually, he is clicking on the button to transfer his bitcoins to the attacker.

For the detail on how such click-jacking attacks are possible and how to carry out a click-jacking attack, I would suggest that you look it up in the Internet. There're some really good sources, for example Portswigger Academy. I think I shouldn't explain it in too much detail because you might get lost easily, and it's not too important for our purpose in general. For now, we only need to know that the web appliation is vulnerable to click-jacking attack. I also put a Proof of Concept there. Do you see the Click button here, it's quite similar to the one that I told you before "Click on the button below the get your free iPhone 15". If you click on that button, it says "You've been click-jacked" which indicates that the web application is vulnerable to click-jacking attacks.

So below are the countermeasures one can take to prevent click-jacking attacks. You can take advantage of the Content Security Policy and use the frame-ancestors directive to prevent click-jacking. Or you can use the X-Frame-Options header to prevent click-jacking. Well, they are alternatives, so you might want to have one or them, or both if you like.

Yeh that's it for the click jacking attack. Any question?

-----------------------------

So the next one: HTTP Strict-Transport-Security Header

Okay, Strict-Transport-Security header is an HTTP header which is normally included in the server responses, as you can see in the screenshot.

The Strict-Transport-Security Header instructs the browser to always use HTTPS to access a website, even when users try to access via HTTP. So this header helps to prevent certain types of attacks, such as man-in-the-middle attacks.

In the Strict-Transport-Security header we can specify the desired duration for the HTTPS enforcement, using the max-age parameter. In this case, it is roughly 2 years. So it should be okay. However, I also recommend to use the includeSubDomains directive to enforce this HTTPS policy to subdomains of the web application. Maybe it's not necessary for now, because we don't have any subdomains for the web application. But it might be necessary in the future when we have subdomains. So it is recommended to set the 'includeSubDomains' directive. This finding is only informational, you can decide to address it or not. Well to be honest, it's not so important for now, because, as I said, we don't have any subdomain for the web application. But it might become important in the future.

-----------------------------

Okay the next one: Missing account deletion feature

During testing the web application, I didn't find the opportunity to completely delete my own account and erase the associated data. I'm not sure if it's important for us to implement this feature because our application stores personal health data and I think there're standards and regulations that requires such an application to allow users to completely delete their account and the associated data. So this finding is just informational so that you Lucas can consider whether it is necessary to implement this feature. Yeah that's it.

-----------------------------

The next one is more interesting: Inclusion of external scripts

So during testing, I noticed that the web application used scripts from external sources, without any additional protection in place. That could be somehow dangerous because, you know, the external scripts can be updated or be compromised and introduce new security vulnerabilities such as XSS. So if we blindly use external scripts and don't check them for security, it could become sometime dangerous. Okay, I think you got the point. So now what can we do? We have two options: the first one is Subresource Integrity or SRI. SRI is that we ensure the integrity of external scripts by checking the hash of them to see if they still have the same hash. So that's one way to check if the scripts have been modified or not. Of course, if using SRI, we need to maintain and update the hashes when the scripts are updated.

Another countermeasure is using Iframe with Sandbox attribute. Well, the Sandbox attribute will limit the capabilities of the iframe, and therefore the capabilities of the external scripts. You should look it up on how to do it specifically on the Internet because it's not written in detail here. I just give you some ideas to start with.

-----------------------------

Okay so let's move to the next one which is a real security issue: Weak password policy

So I don't think I need to explain about it any further because it's clear from the title: the web application enforces a weak password policy. So for example I could set my password to 12345678 or even password. And the web application allowed that. That didn't seem any good to me. Because, you know, there're lots problems with those types of passwords. On the one hand, those well-known passwords are published in many databases of leaked passwords. On the other hand, those passwords are too easy to guess, or to be brute forced. It will take an attacker only some minutes to break those passwords. So it's definitely a security issue.

And the countermeasure is obviously that we enforce a strong password policy, which might include the following points. Yeah I'm not gonna read them all out. They're just for your preference. You can have look at them when you're thinking about how to set up a stronger password policy.

-----------------------------

So I told you that brute-forcing the passwords is possible right? But how can we mitigate brute-forcing? Yeah we use account lockout. For example, we lock out an account after 5 failed login attempts. And indeed, the web application has a such account lockout mechanism. I checked the account lockout mechanism and it turned out to be a good implementation, yeah because we use the authentication service implemented by Google. So it must be well-tested. However, there're some situations where account lockout can be problematic, for example by Email or Username enumeration.

In our web application, if we use a valid username but a wrong password to log in, after 5 attempts, the web application will say that the account has been blocked because of failed attempts.
If we use an invalid, non-existent username, the web application will never say that. It just says: invalid username or password. And the account will never be blocked, yeah because the username doesn't exist. So using this technique, an attacker can determine which username are valid within the web application and can find as much usernames as he can. After having a long list of valid usernames, the attacker can try brute-forcing the weak passwords thanks to the weak password policy as discussed above. Or more efficiently, the attacker can carry a password spraying attack, where he try out a common password for all usernames, and then another common password for all usernames, and so on. In that way, he can avoid the accounts being blocked.

Yeah so in short, by exploiting the account lockout mechanism, the attacker can carry out username enumeration, and get a list of all possible usernames. You got the idea?

So what are the countermeasures? The easiest one is that we use a uniform error message for all cases, for example "Incorrect login details or the account has been locked due to multiple failed attempts. Please try again later or contact support". If that was the case, the attacker wouldn't be able to determine if a username is valid or not, and we can prevent username enumeration.

There're many other countermeasures which you can considers. I put them there for your preference

-----------------------------

Okay so the last one for today: Outdated software version

Well, simply put, the web application uses software that is outdated and should be updated. Outdated softwares can be problematic because they contains security vulnerabilities.

In our case, the web application uses nextjs version 12.3.4 which has the following CVE as you can see. Countermeasure is easy, just update the software component.

-----------------------------

The last one about information disclosure is not fully completed. I haven't finish testing the rest yet. So it's gonna be updated. But yeah, I think that's it for today.  I'll continue working on testing the fastAPI and show you the rest in the next meeting. 

Thank you for your hearing. Admir, you can take over now?



I've have sore throat for a few days, because of the weather in Vietnam. So I will speak quite badly. So if you cannot hear me well, just ask me to repeat okay?

I just have some questions regarding the fastAPI. There're some parameters that I don't understand or don't know where to get them, for example the parameter id, start, end. 

Can we go through all of these API requests and you explain what they are, and what the parameters are, and where I can get some exemplary values for them?

And here what are the parameters key_list, and side. Where can I find some exemplary values?

And here, is that the same parameter id as in the request before??

side: l (1) /r (0)
Start/end: should not be zero because raw is too much => overflow


