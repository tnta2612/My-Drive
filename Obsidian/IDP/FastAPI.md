


# Information Disclosure

Information disclosure can be highly valuable to an attacker for several reasons:

1. **Attack Surface Analysis**: Knowing what information is publicly available about a target system or organization allows attackers to assess the potential attack surface. This helps them identify potential vulnerabilities, weak points, or avenues for exploitation.

2. **Exploiting Vulnerabilities**: Information disclosure often reveals details about the software, hardware, or configurations used by the target. This information can be crucial for identifying specific vulnerabilities or misconfigurations that can be exploited to gain unauthorized access or execute attacks.

3. **Social Engineering**: Information disclosed about individuals within an organization, such as their roles, responsibilities, or contact details, can be exploited for social engineering attacks. Attackers can use this information to craft convincing phishing emails, impersonate trusted individuals, or manipulate targets into revealing sensitive information.

4. **Building Target Profiles**: Aggregated information from multiple sources of disclosure can help attackers build detailed profiles of target organizations or individuals. This includes information about infrastructure, technologies, personnel, business processes, and even personal habits, which can aid in crafting highly targeted and effective attacks.

5. **Reconnaissance for Future Attacks**: Information disclosure is often part of the reconnaissance phase of an attack. By gathering as much information as possible about the target, attackers can plan and execute more sophisticated and targeted attacks in the future, potentially with greater success and less chance of detection.

Overall, information disclosure provides attackers with valuable insights and resources that can be leveraged to launch various types of cyber attacks, ranging from relatively simple exploits to highly sophisticated and targeted campaigns.

## Finding:

HTTP response headers include name of the server in use: ```nginx/1.18.0 (Ubuntu)```

## Criticality:

Informational
CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:N/VI:N/VA:N/SC:N/SI:N/SA:N

## Countermeasures:

1. Adjust the server configuration to modify or remove the "Server" HTTP response header. This can be done by configuring the web server software or application framework to omit this header entirely or provide a generic or obscured value instead of revealing specific server software.

2. **Reverse Proxy**: Implement a reverse proxy server in front of the web server to intercept and modify the HTTP response headers before they reach the client. The reverse proxy can be configured to alter or strip the "Server" header to prevent disclosure of specific server software information.



# Outdated software Version

Outdated software versions refer to instances where software applications or systems are running on older, potentially unsupported versions that lack the latest updates, patches, and security fixes. These outdated versions pose significant security risks, as they may contain known vulnerabilities that could be exploited by attackers to compromise the integrity, confidentiality, or availability of the system. Upgrading to the latest software versions is essential for maintaining a secure and resilient computing environment, as it ensures that critical security patches are applied, reducing the likelihood of successful cyberattacks and data breaches.
## Finding:

The FastAPI uses the following outdated software:
- **nginx** **1.18.0**, which has the following vulnerabilities: https://www.cybersecurity-help.cz/vdb/nginx/nginx/1.18.0/

## Criticality:

Informational
CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:N/VI:N/VA:N/SC:N/SI:N/SA:N

## Countermeasures:

1. Update the softwares in use.



# Exposure of internal information

## Finding:

Traceback information is exposed to the end-users upon encountering an error. The leaked information includes file paths, library details, function calls, and API query details. This vulnerability was triggered during a failed API call, which resulted in a detailed error response being sent to the client. The exposure of traceback and error details can provide attackers with insights into the backend structure and implementation details of our application. This information can be used to craft targeted attacks, potentially leading to further exploits.

![[Pasted image 20240418075513.png]]
![[Pasted image 20240418080416.png]]

## Criticality:

6.9 / Medium
CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:L/VI:N/VA:N/SC:N/SI:N/SA:N

## Countermeasures:

1. Update the error handling logic to prevent detailed tracebacks from being sent to users. Ensure that errors are logged internally instead of being disclosed.



# Unauthenticated reading of measurement data

## Finding:

The FastAPI allows unauthenticated users to access and read the (raw) measurement data of any user registered in the database. This unauthorized access poses a critical risk as it involves potential exposure of personal data, which could lead to privacy violations and misuse of personal information. The breach of such data can lead to several adverse effects including identity theft, loss of trust, reputational damage to the organization, and potential legal implications considering data protection laws and regulations such as GDPR.

![[Pasted image 20240418074845.png]]

![[Pasted image 20240418080115.png]]

![[Pasted image 20240418080501.png]]

## Criticality:

8.7 / High
CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:N/VA:N/SC:N/SI:N/SA:N

## Countermeasures:

1. Temporarily disable public access to the measurement data endpoints (or to the API) until the vulnerability is patched.

2. Developers should revise the current authentication mechanisms to ensure that access to sensitive data requires authentication. Implement and enforce strong authorization checks to make sure that cross-user reading of measurements is not possible.



# Unauthenticated writing of raw measurement data

## Finding:

Sensors can write raw measurement data to the database without any form of authentication. This flaw allows potentially malicious entities to insert fabricated or harmful data into the system, which can lead to data integrity and reliability issues. The ability for unauthenticated entities to write data poses significant risks:
- **Data Integrity:** Data may be tampered with or falsified, affecting decision-making processes reliant on this data.
- **System Reliability:** Erroneous data could trigger incorrect operations, potentially leading to system failures or unsafe conditions.
- **Reputational Damage:** Compromised data integrity could erode user trust and damage our company's reputation.
- **Regulatory Non-Compliance:** Violations of data protection regulations may occur, leading to fines and legal action.

This vulnerability is especially critical within a medical system. If the raw data is manipulated to contain harmful information, the resulting insights derived from it will be inaccurate. Consequently, the company may inadvertently offer false insights to users, potentially endangering their health and well-being.

![[Pasted image 20240418085929.png]]

## Criticality:

9.2 / Critical
CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:N/VI:H/VA:N/SC:N/SI:H/SA:N

## Countermeasures:

1. Temporarily disable the API endpoint used for writing measurement data until authentication measures are implemented.

2. Develop and deploy an authentication mechanism that verifies the identity of the data source before accepting data writes. This may include API tokens, certificates, or IP whitelisting.


# Unauthenticated deletion of measurement data

## Finding:

The FastAPI allows unauthenticated users to delete measurement data. This flaw can be exploited by anyone who can generate or guess a valid measurement ID, enabling them to potentially delete all measurement data stored in the database. This vulnerability poses a severe threat to the integrity and availability of user data. Unauthorized data deletion could lead to significant data loss, affecting user trust and our company's reputation. Furthermore, this issue could potentially have legal and regulatory implications due to non-compliance with data protection standards.

This vulnerability is of utmost concern as it permits any internet user to delete all of the company's measurement data. If there is no database backup in place, the company will be compelled to restart the process of gathering measurement data from its users. Exploiting this vulnerability would have devastating consequences, resulting in significant financial expenses for the company.

![[Pasted image 20240418081514.png]]

## Criticality:

9.2 / Critical
CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:N/VI:N/VA:H/SC:N/SI:N/SA:H

## Countermeasures:

1. Back up the databases regularly

2. Temporarily disable public access to the deletion endpoints (or to the API) until the vulnerability is patched.

3. Implement rigorous authentication and authorization checks on all API endpoints that modify or delete user data. The FastAPI should require authentication tokens for all deletion requests, and verify that the authenticated user has the right to delete the specified measurement data.

