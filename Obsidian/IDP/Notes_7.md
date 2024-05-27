Hi, how are you doing?

Admir, if you have anything to tell, you can go first.

I had a bad hair day yesterday. You know I was in Vietnam for more than one month. When I was at my company 3 days ago, they told me that I need to have my working laptop reset because I brought it along with me to Vietnam. So I gave it to the IT Service Desk of my company. And I should have picked it up yesterday. But after the reset, I cannot log in into my working account. I had no idea what happened. So the people at the IT Service Desk reset my account password but they told me to wait for 24 hours until the password is actually reset. So I have been unable to use that laptop so far. Then yesterday I needed to spend time setting up my girlfriend's old laptop so that I can meet you guys today.

And before I had my working laptop reset at the IT Service Desk, I had backed up all the data on it. I was pretty sure that everything was backed up. I didn't check every detail because I had so many data on my laptop. But then when I tried retrieve the data back from the backup. I found out that the folder, where I've stored the documents for this IDP, it was not there. It wasn't backed up. I was in panic. The deadline is coming. I don't want to do it all again. So I tried to get some files back. Fortunately, I've also stored most of the IDP documents in an Obsidian vault, which I've also synced to my Google Drive. So I was able to get most of the documents back. But, do you still remember to security requirement list in an Excel file that I showed you? The security requirements I extracted from the standards. It wasn't synced to my Google Drive. So I lost it. Yeah I will have to do it again. But still, it's much better than losing all the documents. And that all was also the reason why I couldn't send you the security concept as I promised, Admir. I'm so sorry.

Anyway, thanks for listening to my complaints to myself. Let's get started with main points today:


-------------------------------------------------------------------

Okay let's begin with the sensors. There isn't much thing to discuss on the sensors, because most of the interactions with the servers are through the FastAPI. And I tested it already before. So here are just some informational findings.

So the first one is Bluetooth Static Passkey. Well as you know, we need a passkey to be able to connect the sensors to the mobile app. And the passkey is static, which means it will never change. The static passkey comprises of 6 digits at the moment, which is really easy to be compromised, or bruteforced. And after being compromised, the attacker can gain unauthorized access to the sensors. And because the key is never changed, the attacker will have a long window of opportunity for exploitation. So my suggestion is that, we should change the passkey periodically or on demand. On demand means that we allow users to set there own passkey, and allow them to reset the passkey when they forget it. And that would prevent the risk of the passkey being compromised.

The next findings is also related, and only for informational purpose. As I said, the passkey comprises of 6 digits at the moment, which also means a limited character set. We should allow users to use digits, lower case, upper case, and special characters to set there own passkey. And that would allow for a strong passkey and prevent the risk of the passkey being compromised.

Yeah the last one is Method Confusion Attack as we discussed earlier. I just put it there for completeness. We don't need to discuss it. 

-------------------------------------------------------------------

Let's move on to the more interesting part: the mobile application. And again let's start with some informational findings. And the first one is Source Code Obfuscation. Well, Source Code Obfuscation is a technique to make the source code more difficult to understand, however reserving the whole functionality of the application. And as the result, the obfuscated APK file will be much harder for an attacker to reverse engineer. Well the goals of Source-Code Obfuscation is to protect intellectual property and enhance security of the software application by preventing reverse engineering.

If there's no source code obfuscation in place, it will be much easier for an attacker to reverse engineer the app and he can easily see the application structure, the control flow and all other things. In other words, the source code of the application will be exposed.

That's why I asked you, Lucas, if you have used any kind of source code obfuscation for the mobile app? 

Yeah I know, that's why I was able to reverse engineer the app. Yeah, you can consider to apply some kind of source code obfuscation if necessary.

-------------------------------------------------------------------

So the next finding is also informational, which simply says that you allow the application to be installed on much older Android versions. The current Android version is 14, if I remember correctly. But the Manifest file of the application allows the app to be installed on Android 5 upwards. Older Android versions also means that they do not receive regular security updates anymore. So the app would be vulnerable to exploitation when being installed on those outdated Android versions.

So my suggestion is to increase the required minimum Android version to Android 10, which is API 29, to better protect the application.

-------------------------------------------------------------------

Okay the next findings is also rather informational: Application signed with Debug Certificate

Yeah I know that this APK is a test version of your application, therefore you signed it with a Debug Certificate. This finding here is just for reminding you that Debug Certificate should not be used in production, because it can bring many problems and I'm sure you don't want to have them.

-------------------------------------------------------------------

So combining the two findings above, we have another finding: Janus Vulnerability in Signature Scheme.

Okay so let's me first explain the Janus vulnerability. So you know, the mobile application is signed with a certificate to protect it from being tempered or modified by malicious actors. The Janus vulnerability is a vulnerability that allows an attacker to prepend his malicious code to the application without invalidating the signature. Well, in this way, the attacker can get his malicious code executed.

So the question is if our mobile application is vulnerable to this attack. The answer is, it is vulnerable if the application is installed on an old Android version. So Android versions from 5.0 to 8.0 would be vulnerable if the application was signed only with the v1 signature scheme. But our application is signed with v1 and v2 scheme, so it is vulnerable on Android 5.0 and 7.0.

So that's why allowing the application to be installed on an old Android versions could be really problematic.

-------------------------------------------------------------------

Okay so the next finding: Application Data Backup Enabled.

So as you know, mobile applications can store data on the device. These data can be personal information, login credentials, or any kind of confidential data. So if we allow these data to be backed up, an attacker can use a tool called ADB and USB-debugging to back up the data into his PC, and then analyse the data. If the data is not encrypted, the attacker will have access to sensitive information.

So how to know if the application allows backup? Yeah we can check and allowBackup flag in the manifest file. So this flag wasn't set in the manifest file of the application. And the default value will be enabled, which allows for backup of data to be carried out.

So my recommendations are that
First, we should explicitly set the allowBackup flag, either to True or False, depending on the business requirements. If backup is not desired, we should explicitly set it to False
Second, we should encrypt the sensitive data that is stored on the device. Well, to this point, Lucas, I need you to be honest, do you use any kind of encryption for the data that is stored on the device?

-------------------------------------------------------------------

Oh really, because I did find some sensitive data and I need you to confirm whether they are encrypted or not. Okay let's see.

Yeah, I know that because I did find some sensitive data. Okay let's move on to the last two findings.

The first one: Exposure of AES key in Plain-Text XML file.
So among the data that the application stores on the device, I found a file named "FlutterSecureKeyStorage.xml". So I tried to have a look at it. So the file contains a string name and a value after that. And the string name seems to be base-64 encoded. So I tried to base-64 decode the string name, which gave me the string "This is the key for a secure storage AES key". So I'm pretty sure that the value behind this string name is the actual AES key used for encryption, right? 

So the key is exposed.

-------------------------------------------------------------------

Additionally, I also found some Access tokens and User Data. Well, I also found this file, which is rather illegible. I tried to decode it. Well first I had to HTML-decode the string value, which give me a JSON, containing all the user details, the refresh token, and the access token. Well the user details are exposed.

Then I tried to understand what the access token is. It turned out to me that the access token is a JSON Web Token. This token contains various information and is used to authenticate against the server. So this token is exposed. If an attacker have access to this token, he will be able to authenticate against the server, and do all the things that the user can do.

That's possible because the tokens are not encrypted.

So based on the two findings above, I guessed that you're either not using any kind encryption for the data stored on the mobile device, or the data encryption mechanism is not comprehensive enough.

So my recommendation is that we should ensure sensitive information stored on the device is encrypted.

-------------------------------------------------------------------

Yeah that's it for today. Thank you for listening.


Yeah I think my work is almost done. I'll take the remaining time to write the final report and prepare the presentation.

And I also need to create the security requirement excel file again. I'll will ask Niklas for a time slot for the presentation after Admir more or less finishes the security concept.


