


# Source Code Obfuscation

Source code obfuscation is a technique used to obscure or make code difficult to understand, without altering its functionality. It involves transforming the source code of a software application into a form that is challenging for humans to comprehend, while still allowing the code to be executed by computers as intended. Obfuscation is typically applied to software written in high-level programming languages such as Java, C/C++, and JavaScript. The primary goal of source code obfuscation is to protect intellectual property, prevent reverse engineering, and enhance the security of software applications. 

## Finding:

The absence of source code obfuscation makes the mobile application susceptible to reverse engineering and unauthorized access. Attackers might easily analyze the code structure, extract sensitive information such as API keys, encryption algorithms, and proprietary algorithms, and exploit vulnerabilities present in the application.

## Criticality:

Informational
CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:N/VI:N/VA:N/SC:N/SI:N/SA:N

## Countermeasures:

1. Integrate source code obfuscation techniques into the development pipeline to obfuscate sensitive parts of the codebase, including critical algorithms, API endpoints, and authentication mechanisms.
2. Utilize industry-standard obfuscation tools and techniques to obscure the application's logic and prevent reverse engineering attempts effectively.



# Installation on Vulnerable and Unpatched Android Versions

Android devices receive security updates and patches from Google to address vulnerabilities and enhance the overall security of the operating system. However, older Android versions, such as 5.0 to 5.0.2, may not receive regular security updates or patches, leaving them vulnerable to exploitation by malicious actors. These vulnerabilities can be exploited to compromise the confidentiality, integrity, and availability of user data and device functionality.

## Finding:

The application's minimum SDK requirement of 21 allows it to be installed on Android versions 5.0 to 5.0.2, which lack reasonable security updates from Google. Devices running these outdated Android versions are at risk of exploitation by various threats, including malware, remote code execution, and privilege escalation attacks. The absence of security updates increases the likelihood of successful exploitation and compromises the overall security posture of the device and its users.

![[Pasted image 20240513092053.png]]
## Criticality:

Informational
CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:N/VI:N/VA:N/SC:N/SI:N/SA:N

## Countermeasures:

1. Increase the minimum SDK version requirement of the application to a version that ensures compatibility with Android versions receiving reasonable security updates from Google. Consider raising the minimum SDK version to support Android versions 10 (API 29) or later, which receive regular security updates and patches.



# Application Signed with Debug Certificate 

In software development, certificates play a crucial role in ensuring the authenticity and integrity of applications. Certificates are used to sign applications, indicating that they have been verified and approved by the developer or organization responsible for their creation. This process helps establish trust between users and applications, mitigating the risk of unauthorized or malicious software.

However, debug certificates are distinct from release certificates and are intended for use during the development and testing phases of application creation. Debug certificates offer convenience for developers by allowing them to install and test applications on their devices without the need for official signing processes. While this feature expedites development, it introduces significant security vulnerabilities if deployed in production environments. Debug certificates lack the security measures and assurances provided by release certificates, making the application susceptible to various threats, including:

1. **Unauthorized Modifications:** Debug-signed applications are easier to tamper with, as they lack the cryptographic protections afforded by release certificates. Malicious actors could potentially modify the application's code, injecting malicious payloads or backdoors without detection.

2. **Man-in-the-Middle Attacks:** Applications signed with debug certificates may be more susceptible to interception and tampering during transmission. Attackers could exploit this vulnerability to intercept communication between the application and external servers, compromising sensitive data or injecting malicious code.

3. **Unauthorized Access:** Debug-signed applications may expose sensitive debug information, such as log messages and stack traces, to end-users. This information could aid attackers in identifying and exploiting vulnerabilities within the application.

## Finding:

The application has been signed with a debug certificate, indicating that it was likely developed and tested using debugging tools and environments. While this may be acceptable during the initial stages of development, shipping a production application with a debug certificate poses severe security risks. 

![[Pasted image 20240513091145.png]]
## Criticality:

Informational
CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:N/VI:N/VA:N/SC:N/SI:N/SA:N

## Countermeasures:

1. Ensure that all production applications are signed exclusively with release certificates obtained from trusted certificate authorities. Release certificates provide the necessary security assurances for production deployments, safeguarding against unauthorized modifications and tampering.



# Janus Vulnerability in Signature Scheme

Android applications are signed with cryptographic signatures to ensure their authenticity and integrity. The Android operating system verifies these signatures to confirm that applications have not been tampered with or modified by malicious actors. However, vulnerabilities in the signature verification process, such as the Janus vulnerability, can be exploited to bypass these security measures and inject malicious code into legitimate applications.

The Janus vulnerability allows attackers to prepend malicious code to legitimate Android application packages (APKs) without invalidating their signatures. By manipulating the APK file's metadata, attackers can deceive the Android operating system into executing both the original application code and the appended malicious code. This exploitation technique enables various types of attacks, including data theft, privilege escalation, and remote code execution.

## Finding:

Android versions 5.0 to 8.0 are particularly vulnerable to Janus attacks when applications are signed solely with the v1 signature scheme. Additionally, applications signed with v1 and v2/v3 schemes on Android 5.0 to 7.0, are also susceptible to Janus attacks, amplifying the scope of this vulnerability.

## Criticality:

8.5 / High
CVSS:4.0/AV:L/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N

## Countermeasures:

1. Sign applications with both v1 and v2/v3 signature schemes to mitigate the Janus vulnerability across a broader range of Android versions. Leveraging multiple signature schemes enhances the resilience of the application's signature verification process and reduces the risk of exploitation.

2. Update the target SDK version of the application to ensure compatibility with newer Android security features and protections. Targeting the latest SDK version allows the application to benefit from security enhancements and mitigations against known vulnerabilities, including Janus attacks.



# Application Data Backup Enabled

Android applications can store sensitive user data, including personally identifiable information (PII), login credentials, and other confidential data. To protect this information, Android provides mechanisms for securing application data, including the ability to disable data backups. By default, Android allows applications to be backed up via adb, which can pose security risks if sensitive data falls into the wrong hands.

## Finding:

The absence of the `[android:allowBackup]` flag in the application's manifest file leaves the default backup behavior enabled, potentially exposing sensitive user data to unauthorized access. This configuration allows anyone with USB debugging enabled to extract application data from the device using adb, even without user consent.

## Criticality:

5.1 / Medium
CVSS:4.0/AV:P/AC:L/AT:N/PR:N/UI:N/VC:H/VI:N/VA:N/SC:N/SI:N/SA:N

## Countermeasures:

1. Explicitly set the `[android:allowBackup]` flag to false in the application's manifest file to disable data backups via adb. This prevents unauthorized access to sensitive user data stored by the application and enhances data security and privacy.

2. Encrypt sensitive user data stored by the application to protect it from unauthorized access, even in the event of a data backup. Utilize strong encryption algorithms and key management practices to safeguard data confidentiality and integrity.



# Exposure of AES Key in Plain-Text XML File

The SharedPreferences API is frequently utilized for persistently storing small collections of key-value pairs. Data saved within a SharedPreferences object is typically written to plain-text XML files. These files can be configured as either world-readable (accessible to all apps) or private. It's crucial to examine these files to determine if any sensitive data is inadvertently exposed.

## Finding:

During an examination of the application "tech.eversion.measurement.test," it was discovered that an AES key for secure storage is stored in a plain-text XML file. Specifically, the AES key was found within the file "FlutterSecureKeyStorage.xml" located in the directory "/data/data/tech.eversion.measurement.test/shared_prefs/". 

![[Pasted image 20240514102838.png]]
![[Pasted image 20240514102910.png]]

The exposure of the AES key in a plain-text format poses a significant security risk to the application and its users. Encryption keys are intended to be kept confidential, and their exposure can lead to unauthorized access to sensitive data. With access to the AES key, an attacker could potentially decrypt any data encrypted using this key, thereby gaining access to sensitive information.

## Criticality:

6.9 / Medium
CVSS:4.0/AV:L/AC:L/AT:N/PR:N/UI:N/VC:H/VI:N/VA:N/SC:N/SI:N/SA:N

## Countermeasures:

1. The AES key stored in the plain-text XML file must be immediately removed or replaced with a more secure storage mechanism. Consider generating a new AES key and securely storing it using appropriate encryption mechanisms or secure storage solutions provided by the operating system or third-party libraries.

2. Review and update the application's data storage practices to ensure that sensitive information, including encryption keys, is securely stored and protected from unauthorized access. Utilize secure storage solutions provided by the Android platform, such as Android Keystore, for storing encryption keys securely.


# Exposure of Access Token and User Info

The SharedPreferences API is frequently utilized for persistently storing small collections of key-value pairs. Data saved within a SharedPreferences object is typically written to plain-text XML files. These files can be configured as either world-readable (accessible to all apps) or private. It's crucial to examine these files to determine if any sensitive data is inadvertently exposed.

## Finding:

The application's sensitive data, including access tokens and user information, is stored in an XML file within the app's data directory "/data/data/tech.eversion.measurement.test/shared_prefs/com.google.firebase.auth.api.Store.W0RFRkFVTFRd+MTo1ODU5NTU0NzM3MzM6YW5kcm9pZDozZTAyZDNjNTdmNDU2ZTg2OTNhNjJj.xml". 

User information, including email addresses, user IDs, and authentication provider details, is stored in plaintext within the file. Revealing such information can lead to identity theft, phishing attacks, and unauthorized account access.

![[Pasted image 20240514105155.png]]
![[Pasted image 20240514105257.png]]

The access token included in the file is a JWT (JSON Web Token), which can grant access to protected resources on behalf of the user. Exposing access tokens can lead to unauthorized access to user accounts and sensitive data.

![[Pasted image 20240514105354.png]]

## Criticality:

6.9 / Medium
CVSS:4.0/AV:L/AC:L/AT:N/PR:N/UI:N/VC:H/VI:N/VA:N/SC:N/SI:N/SA:N

## Countermeasures:

1. Ensure that sensitive data such as access tokens and user information is securely stored using encryption or secure storage mechanisms provided by the platform.

2. Implement robust token management practices such as token expiration and revocation to mitigate the risk associated with exposed access tokens.