Bluetooth LE Security Level 4:
=> risk: Method Confusion Attack


Regarding the bluetooth pairing process between the mobile device and the sensor that you guys discussed last week. And because I don't have the sensor with me yet, I'd like to ask one more question to make sure that I understand it completely. 
So Lucas as you wrote in the Teams chat, the sensor doesn't have a display, so it's gonna pair with the mobile device using Passkey Entry process, where the mobile device displaying a 6-digit key, and we entering that key into the sensor? So that means the sensor has the input capability right, in form of a key pad I guess, or what?

And what do you mean with we using a static 6-digit key stored in the SoC Flash of the Sensor Board. Why is it static? As far as I understand, in the Passkey Entry process, each time we pair the devices, a new key is dynamically and randomly generated. So what do you mean with the static-key stored in the Flash? Is it there before the paring, or after the pairing?

Regarding the bluetooth paring between the devices, I don't think we can change much there, because the processes are standardized and I don't think we're capable of changing or modifying the paring protocols. As long as we use the one that offers the most security, that should be okay. In our case, Lucas said it is Security Level 4, which offers the strongest security. I think it's fine for us. I just wanted to say that so that you don't waste your time analyzing the paring protocol or analyzing how Bluetooth Low Energy works. We will not be able to modify anything there, because they are all standardized processes.

Btw, if you're interested in the bluetooth paring protocol, there's one thing that might interest you guys. I don't know if you've ever heard of the Method Confusion Attack on the Bluetooth paring. It's quite a new attack in the bluetooth paring protocol. And there's still no mitigation for it yet, because for that they need to change the paring protocol, which is not easy and takes lots of time. 



Admir, can you share your document with me? I find it really good. I also want to have a look at it latter. And maybe we can have some discussions on it.

In general, I find it great that you considered every aspect of the system. I just have one minor feedback on the way you approach it. I find it good that you ask Lucas things that you don't know or you're not sure about. Like whether HTTPS is used, which cipher suite is used. And you use that information to evaluate the system. I find it great. But you might miss something. That is, the answers from Lucas might not be correct. I know that Lucas knows a lot about the system. He knows how it works. But, we need to check it if they are really correctly implemented. For example, Lucas told you the server uses this cipher suite. But we also need to check if the server really uses it or if the server also offers any different cipher suite. I mean, in practice, we need to somehow check it to see if it is really correct. And we shouldn't rely only on the answers of Lucas. Sorry Lucas, I didn't mean something bad about you. But that's really the job of security engineers to find out how things are indeed implemented.

If you guys have interest in the Method Confusion Attack on the Bluetooth paring protocol, I can briefly show you what it is about. Do you have interest or not?

I suppose that you guys already know the different ways of paring in Bluetooth Low Energy, well Passkey Entry, Numeric Comparison, Just Works. I'm not gonna explain it further.

Okay now comes the boring theoretical part, in the Method Confusion Attack, the attacker tries to become the Man-in-the-Middle, sitting between two devices, reading and modifying the traffic. So first of all, the attacker exchanges his IO capabilities and the public keys with the sensor and the mobile device in a way that it forces the mobile device to perform Numeric Comparison with it, and the sensor to perform Passkey Entry with the attacker.
So on the attacker's device and the mobile device, both will display a key Va which the user should check if it is the same on both device. Then the attacker use this key V_a to perform Passkey Entry with the sensor. The attacker display this value V_a, and the user inputs this value into the sensor. After this process, the attacker becomes the MitM between the mobile device and the sensor. Because the attacker now knows the Long-term-Keys to the mobile device and to the sensor, the attacker can now read and modify any traffic between the two.

This attack is still relevant today and there's still no mitigation implemented for it in practice. For the mitigation, the pairing protocol itself must be changed, and it definitely takes some time until there.
So for this risk, it's really likely that we have to accept the risk because modify the paring protocol is out of scope for us. 


So in other words, we might not be able to solve this. But I just wanted to show it to you so that you know that such an attack exists on the Bluetooth paring protocol. And the risk exists in practice.

Yeah, otherwise I don't have anything more to show you. Just wanna ask, is there anything from the last meeting that I should know?

I tried to access the Influx database this morning and it required credentials to sign in? Can I get the credentials for it somehow?

Do you have more information on the REST API? Like, which HTTP requests I can send to it and which responses I am supposed to receive. => /docs

Can you show me around the Firebase setup? Particularly, the part of the Firebase setup that you think the most relevant for us to investigate in terms of security. I just look around the our Firebase project and it was almost empty, unpopulated, except for some entries for authentication and databases. Apart from that I didn't see any security settings or configurations or anything related to security.



eversion
Eversion2023!



