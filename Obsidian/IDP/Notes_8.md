I only have one interesting finding to show you today. It's about the web application. 

Admir, if you don't mind, I'd like to go first because I'm gonna have another meeting at 12. I just need, like, 5 minutes and then you can take over.

Okay just one finding that I found out a couple days ago. I name it "Phishing mails that impersonates Eversion Technologies."

Okay so what is it about. So, you know that the web application features an input form allowing users to register their emails for news updates. You can see it on the right. So users need to provide their first names, last names, and email addresses. And after that, a confirmation email from Eversion Technologies will be sent to the given email address to confirm the user's registration. This confirmation email contains the first name of the user.

While playing around with input form, I found out that the length of the first name is not limited, which means we could write anything into that field, as you can see in the screenshot below. And that can be easily exploited to insert malicious content into the confirmation email that will be sent to victim. And that can also be exploited to craft a phishing email which will be sent on behalf of Eversion Technologies.

And because the phishing emails are sent by Eversion Technologies, users will simply trust them, click on a link in the email, and give their passwords or sensitive information to the attacker.

Especially here, the attacker can use this input form to send the phishing email to any victim they want. The attacker just needs to provide the email address of the victim in this form and Eversion Technologies will send the phishing mail for him.

So, besides the impact on confidentiality, that users will trust the phishing mails and give the attacker their sensitive information such as passwords, there's also negative impact on the reputation of the company, because the phishing mails are sent on behalf of Eversion Technologies.

So, in my opinion, the criticality of this vulnerability is high. And it should be fixed.

So first We can simply limit the length of this first name.
Then we should also sanitize the content of the input to make sure that malicious content is filtered before the confirmation mail is sent to users.

Yeah that's it. Any question?
I didn't have the time to craft a beautiful phishing email. Partly because this confirmation email is sent only once for each email address. And I used 4 or 5 email addresses to test it. So not I'm like out of email address, I don't have anymore email address to test it further. And additionally, it also take quite a long time until this confirmation mail is sent out. So it's kind of hard to know, when I will receive the confirmation email. So I decided not to test it further. But I believe that crafting a beautiful phishing email in the first name is possible. You can easily use line breaks and insert phishing links and so on.



I'm not sure if it's reasonable for you to include specific details into the security concept. I mean the security concept should be rather abstract and general for all products of the company. So maybe try to think from the management point of view, and keep it abstract and general so that it can be used for all products of the company in the future.

Well think in this way, the security concept should be so general that you can also use it for different products of the company, even in the future. When something comes out or something is fixed, your security concept doesn't need to be fixed.

Well, in my opinion, the security concept is more on the management level, not the implementation level. So it would be better to not include every detailed vulnerabilities there, because they will be fixed and your security concept will not be correct any more. Maybe think about writing.



