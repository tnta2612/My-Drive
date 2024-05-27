# Gitleaks (https://github.com/gitleaks/gitleaks)

GitLeaks is an open-source tool crafted to detect inadvertent exposure of sensitive information within Git repositories. It meticulously scans codebases for potential secrets, such as API keys, passwords, and other confidential data. By promptly alerting developers to potential security vulnerabilities, GitLeaks empowers them to take corrective measures to fortify their repositories. According to GitHub's guidelines on safeguarding API credentials, developers are advised against pushing unencrypted authentication credentials, such as tokens or keys, to any repository, irrespective of its privacy settings. Instead, they are encouraged to leverage GitHub Actions secrets or Codespaces secrets for enhanced security measures (source: https://docs.github.com/en/rest/authentication/keeping-your-api-credentials-secure?apiVersion=2022-11-28).

> [!tip] 
> To view a specific commit on GitHub, replace PROJECT with the project name and HASH with the commit's hash:  https://github.com/ITEversion/PROJECT/commit/HASH
> To view a specific commit using command line: ```git show HASH -L START,END:FILENAME```

After conducting a Gitleaks scan on the code base and refining the results to eliminate false positives and duplicates, the following secrets were uncovered:
#### eversion-fastapi:

Finding:     "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvwIBADANBgkqhkiG9w0BAQEFAASCBKkwggSlAgEAAoIBAQCj843xAnAYUJqf\nfLjBb...-\n",
Secret:      -----BEGIN PRIVATE KEY-----\nMIIEvwIBADANBgkqhkiG9w0BAQEFAASCBKkwggSlAgEAAoIBAQCj843xAnAYUJqf\nfLjBb...
RuleID:      private-key
Entropy:     6.018799
File:        app/credentials.py
Line:        5
Commit:      8f1c6a0b90536fc4959a911097eddaaf297e434e
Author:      Lucas-Heitele
Email:       lucas@eversion.tech
Date:        2024-03-03T12:52:09Z
Fingerprint: 8f1c6a0b90536fc4959a911097eddaaf297e434e:app/credentials.py:private-key:5

Finding:     "token": "pNX4dIkHUKUforhFNtLRm7LoKG3nBhRydIfX-1xNJro4P8UxZAojx59CuqllgtKQ_5QssgZn2kGp55md-vA5sg=="
Secret:      pNX4dIkHUKUforhFNtLRm7LoKG3nBhRydIfX-1xNJro4P8UxZAojx59CuqllgtKQ_5QssgZn2kGp55md-vA5sg==
RuleID:      generic-api-key
Entropy:     5.403820
File:        app/credentials.py
Line:        16
Commit:      8f1c6a0b90536fc4959a911097eddaaf297e434e
Author:      Lucas-Heitele
Email:       lucas@eversion.tech
Date:        2024-03-03T12:52:09Z
Fingerprint: 8f1c6a0b90536fc4959a911097eddaaf297e434e:app/credentials.py:generic-api-key:16

![[Pasted image 20240318175239.png]]

Finding:     token='pNX4dIkHUKUforhFNtLRm7LoKG3nBhRydIfX-1xNJro4P8UxZAojx59CuqllgtKQ_5QssgZn2kGp55md-vA5sg=='
Secret:      pNX4dIkHUKUforhFNtLRm7LoKG3nBhRydIfX-1xNJro4P8UxZAojx59CuqllgtKQ_5QssgZn2kGp55md-vA5sg==
RuleID:      generic-api-key
Entropy:     5.403820
File:        app/main.py
Line:        45
Commit:      8f1c6a0b90536fc4959a911097eddaaf297e434e
Author:      Lucas-Heitele
Email:       lucas@eversion.tech
Date:        2024-03-03T12:52:09Z
Fingerprint: 8f1c6a0b90536fc4959a911097eddaaf297e434e:app/main.py:generic-api-key:45

![[Pasted image 20240318175557.png]]

Finding:     "token": "jq9MQCNL2xe_n85MKE7sQB7DfAN6en_jVM91nWHHU7qOfC9--vVwAXSXWVVJOOV1fG6INc3YgurkdtTRunRmaQ=="
Secret:      jq9MQCNL2xe_n85MKE7sQB7DfAN6en_jVM91nWHHU7qOfC9--vVwAXSXWVVJOOV1fG6INc3YgurkdtTRunRmaQ==
RuleID:      generic-api-key
Entropy:     5.517456
File:        app/influxdb-credentials.json
Line:        3
Commit:      147fc243c7b6d7c3eab28841dacf5b03adc60ff2
Author:      Lucas
Email:       lucas@eversion.tech
Date:        2023-07-03T21:25:11Z
Fingerprint: 147fc243c7b6d7c3eab28841dacf5b03adc60ff2:app/influxdb-credentials.json:generic-api-key:3

![[Pasted image 20240319093322.png]]

Finding:     "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQDStDKk8S8M8W0A\nV0+7L...-\n",
Secret:      -----BEGIN PRIVATE KEY-----\nMIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQDStDKk8S8M8W0A\nV0+7L...
RuleID:      private-key
Entropy:     6.009548
File:        app/firestore-credentials.json
Line:        5
Commit:      147fc243c7b6d7c3eab28841dacf5b03adc60ff2
Author:      Lucas
Email:       lucas@eversion.tech
Date:        2023-07-03T21:25:11Z
Fingerprint: 147fc243c7b6d7c3eab28841dacf5b03adc60ff2:app/firestore-credentials.json:private-key:5

![[Pasted image 20240319094746.png]]

4:24PM WRN warning: exhaustive rename detection was skipped due to too many files.
Finding:     token='VFvJgQzXkq0TVb-M4mLMPGHkw0oXv8XASrfGKfZk9j_5rIrp-Lak20Tdb6zTOJPTGuHhLg6AiWsZkyOQcbrnoQ=='
Secret:      VFvJgQzXkq0TVb-M4mLMPGHkw0oXv8XASrfGKfZk9j_5rIrp-Lak20Tdb6zTOJPTGuHhLg6AiWsZkyOQcbrnoQ==
RuleID:      generic-api-key
Entropy:     5.457852
File:        app/main.py
Line:        12
Commit:      7158d086b12b2c1904320e00f8145f7e99def61c
Author:      dazesace
Email:       dazemcblaze@gmail.com
Date:        2022-12-09T16:08:15Z
Fingerprint: 7158d086b12b2c1904320e00f8145f7e99def61c:app/main.py:generic-api-key:12

![[Pasted image 20240319094914.png]]

4:24PM WRN warning: you may want to set your diff.renameLimit variable to at least 1909 and retry the command.
Finding:     token='FgWDJGILv2pAE-KXRl2SV0NY33CMW4lSPTXNY4EkPLRuGi3Ncd8DL9YIQodSowTxgF1J1n0HJ20oXQptD4SnPQ=='
Secret:      FgWDJGILv2pAE-KXRl2SV0NY33CMW4lSPTXNY4EkPLRuGi3Ncd8DL9YIQodSowTxgF1J1n0HJ20oXQptD4SnPQ==
RuleID:      generic-api-key
Entropy:     5.347914
File:        app/main.py
Line:        12
Commit:      fee2e8b382eb9438c30185ca4ff49a5fc564ac3c
Author:      dazesace
Email:       dazemcblaze@gmail.com
Date:        2022-11-22T16:25:21Z
Fingerprint: fee2e8b382eb9438c30185ca4ff49a5fc564ac3c:app/main.py:generic-api-key:12

Finding:     token='6LUgV6MsmKqEsi5uyb24RZWvZKp5SF6IG5BUk0W_NPd1vTjeT4K-7LVy3j9p8AEaGwKXrBEIoYDG5jd5AwjyxQ=='
Secret:      6LUgV6MsmKqEsi5uyb24RZWvZKp5SF6IG5BUk0W_NPd1vTjeT4K-7LVy3j9p8AEaGwKXrBEIoYDG5jd5AwjyxQ==
RuleID:      generic-api-key
Entropy:     5.565918
File:        app/main.py
Line:        10
Commit:      eb1cfd2d2ba2e41b3f4e8baaf7f842764eb1db37
Author:      dazesace
Email:       dazemcblaze@gmail.com
Date:        2022-11-21T23:40:28Z
Fingerprint: eb1cfd2d2ba2e41b3f4e8baaf7f842764eb1db37:app/main.py:generic-api-key:10

Finding:     token='hAriYFOjM2vDS1I0OprGr_LFHlCr9ARizCJQcItGdBSU43R6apau7fgqNzqQQnVCsetN_e1JcbNMb1f46ihBgQ=='
Secret:      hAriYFOjM2vDS1I0OprGr_LFHlCr9ARizCJQcItGdBSU43R6apau7fgqNzqQQnVCsetN_e1JcbNMb1f46ihBgQ==
RuleID:      generic-api-key
Entropy:     5.493300
File:        app/main.py
Line:        14
Commit:      f0c5d32b89748cb829cd2b86064719601f071b95
Author:      dazesace
Email:       dazemcblaze@gmail.com
Date:        2022-11-01T17:10:40Z
Fingerprint: f0c5d32b89748cb829cd2b86064719601f071b95:app/main.py:generic-api-key:14

#### evrMeasure_v1.0

36 commits scanned.
no leaks found.

#### eversion-dashboard

Finding:     token:"pNX4dIkHUKUforhFNtLRm7LoKG3nBhRydIfX-1xNJro4P8UxZAojx59CuqllgtKQ_5QssgZn2kGp55md-vA5sg=="
Secret:      pNX4dIkHUKUforhFNtLRm7LoKG3nBhRydIfX-1xNJro4P8UxZAojx59CuqllgtKQ_5QssgZn2kGp55md-vA5sg==
RuleID:      generic-api-key
Entropy:     5.403820
File:        next.config.js
Line:        21
Commit:      dbc67ba2e229744c89380b555c597a67f3da6298
Author:      EVERSION Technologies
Email:       lucas@eversion.tech
Date:        2024-02-29T04:41:33Z
Fingerprint: dbc67ba2e229744c89380b555c597a67f3da6298:next.config.js:generic-api-key:21

![[Pasted image 20240319095820.png]]

Finding:     apiKey: "AIzaSyBr2d-8aGZI4Egz6upEPjAna1rJleTVUXo"
Secret:      AIzaSyBr2d-8aGZI4Egz6upEPjAna1rJleTVUXo
RuleID:      generic-api-key
Entropy:     4.907072
File:        next.config.js
Line:        12
Commit:      45d08f705d90d6672e625ac106018be404707262
Author:      EVERSION Technologies
Email:       lucas@eversion.tech
Date:        2024-02-29T04:27:17Z
Fingerprint: 45d08f705d90d6672e625ac106018be404707262:next.config.js:generic-api-key:12

![[Pasted image 20240319100346.png]]

Finding:     token:"jq9MQCNL2xe_n85MKE7sQB7DfAN6en_jVM91nWHHU7qOfC9--vVwAXSXWVVJOOV1fG6INc3YgurkdtTRunRmaQ=="
Secret:      jq9MQCNL2xe_n85MKE7sQB7DfAN6en_jVM91nWHHU7qOfC9--vVwAXSXWVVJOOV1fG6INc3YgurkdtTRunRmaQ==
RuleID:      generic-api-key
Entropy:     5.517456
File:        next.config.js
Line:        21
Commit:      678e3093cb35f37acac033a2fa75cebefd10bb33
Author:      Lucas
Email:       lucas@eversion.tech
Date:        2023-02-13T10:17:22Z
Fingerprint: 678e3093cb35f37acac033a2fa75cebefd10bb33:next.config.js:generic-api-key:21

Finding:     const token="jq9MQCNL2xe_n85MKE7sQB7DfAN6en_jVM91nWHHU7qOfC9--vVwAXSXWVVJOOV1fG6INc3YgurkdtTRunRmaQ=="
Secret:      jq9MQCNL2xe_n85MKE7sQB7DfAN6en_jVM91nWHHU7qOfC9--vVwAXSXWVVJOOV1fG6INc3YgurkdtTRunRmaQ==
RuleID:      generic-api-key
Entropy:     5.517456
File:        pages/measurements/[id].js
Line:        54
Commit:      732136f13de8ba88afecdc4d2e9c379ef5a3bb19
Author:      Lucas
Email:       lucas@eversion.tech
Date:        2023-02-13T10:19:21Z
Fingerprint: 732136f13de8ba88afecdc4d2e9c379ef5a3bb19:pages/measurements/[id].js:generic-api-key:54

Finding:     token:"jq9MQCNL2xe_n85MKE7sQB7DfAN6en_jVM91nWHHU7qOfC9--vVwAXSXWVVJOOV1fG6INc3YgurkdtTRunRmaQ=="
Secret:      jq9MQCNL2xe_n85MKE7sQB7DfAN6en_jVM91nWHHU7qOfC9--vVwAXSXWVVJOOV1fG6INc3YgurkdtTRunRmaQ==
RuleID:      generic-api-key
Entropy:     5.517456
File:        src/pages/measurements/_old.js
Line:        54
Commit:      129845916dc60a46db3195c280fce0d837efcdc3
Author:      Lucas
Email:       lucas@eversion.tech
Date:        2023-02-16T20:34:24Z
Fingerprint: 129845916dc60a46db3195c280fce0d837efcdc3:src/pages/measurements/_old.js:generic-api-key:54

Finding:     apiKey: "AIzaSyBxyfKVNCT1BG7pn2r9sHvyX4KhKaMgqwU"
Secret:      AIzaSyBxyfKVNCT1BG7pn2r9sHvyX4KhKaMgqwU
RuleID:      gcp-api-key
Entropy:     4.938998
File:        src/pages/measurements/_old.js
Line:        19
Commit:      129845916dc60a46db3195c280fce0d837efcdc3
Author:      Lucas
Email:       lucas@eversion.tech
Date:        2023-02-16T20:34:24Z
Fingerprint: 129845916dc60a46db3195c280fce0d837efcdc3:src/pages/measurements/_old.js:gcp-api-key:19

Finding:     apiKey: "AIzaSyBxyfKVNCT1BG7pn2r9sHvyX4KhKaMgqwU"
Secret:      AIzaSyBxyfKVNCT1BG7pn2r9sHvyX4KhKaMgqwU
RuleID:      gcp-api-key
Entropy:     4.938998
File:        src/pages/measurements/[id].js
Line:        19
Commit:      678e3093cb35f37acac033a2fa75cebefd10bb33
Author:      Lucas
Email:       lucas@eversion.tech
Date:        2023-02-13T10:17:22Z
Fingerprint: 678e3093cb35f37acac033a2fa75cebefd10bb33:src/pages/measurements/[id].js:gcp-api-key:19

Finding:     token:"jq9MQCNL2xe_n85MKE7sQB7DfAN6en_jVM91nWHHU7qOfC9--vVwAXSXWVVJOOV1fG6INc3YgurkdtTRunRmaQ=="
Secret:      jq9MQCNL2xe_n85MKE7sQB7DfAN6en_jVM91nWHHU7qOfC9--vVwAXSXWVVJOOV1fG6INc3YgurkdtTRunRmaQ==
RuleID:      generic-api-key
Entropy:     5.517456
File:        src/pages/measurements/[id].js
Line:        54
Commit:      678e3093cb35f37acac033a2fa75cebefd10bb33
Author:      Lucas
Email:       lucas@eversion.tech
Date:        2023-02-13T10:17:22Z
Fingerprint: 678e3093cb35f37acac033a2fa75cebefd10bb33:src/pages/measurements/[id].js:generic-api-key:54

Finding:     apiKey: "AIzaSyBxyfKVNCT1BG7pn2r9sHvyX4KhKaMgqwU"
Secret:      AIzaSyBxyfKVNCT1BG7pn2r9sHvyX4KhKaMgqwU
RuleID:      gcp-api-key
Entropy:     4.938998
File:        next.config.js
Line:        12
Commit:      9c7686ee710731754953693722c1660edb98f70a
Author:      Lucas
Email:       lucas@eversion.tech
Date:        2023-02-07T12:41:52Z
Fingerprint: 9c7686ee710731754953693722c1660edb98f70a:next.config.js:gcp-api-key:12

Finding:     token:"VFvJgQzXkq0TVb-M4mLMPGHkw0oXv8XASrfGKfZk9j_5rIrp-Lak20Tdb6zTOJPTGuHhLg6AiWsZkyOQcbrnoQ=="
Secret:      VFvJgQzXkq0TVb-M4mLMPGHkw0oXv8XASrfGKfZk9j_5rIrp-Lak20Tdb6zTOJPTGuHhLg6AiWsZkyOQcbrnoQ==
RuleID:      generic-api-key
Entropy:     5.457852
File:        next.config.js
Line:        21
Commit:      9c7686ee710731754953693722c1660edb98f70a
Author:      Lucas
Email:       lucas@eversion.tech
Date:        2023-02-07T12:41:52Z
Fingerprint: 9c7686ee710731754953693722c1660edb98f70a:next.config.js:generic-api-key:21

Finding:     const _token = "m8ghziB2_7h8hmTQfITpWKp-HNvut62ZqfoKmXf8m09DzwiryCWv9VK0E2F48sLn-vGMG6ICpumPISTdXhz_Wg=="
Secret:      m8ghziB2_7h8hmTQfITpWKp-HNvut62ZqfoKmXf8m09DzwiryCWv9VK0E2F48sLn-vGMG6ICpumPISTdXhz_Wg==
RuleID:      generic-api-key
Entropy:     5.386663
File:        pages/measurements/[id].js
Line:        43
Commit:      c4b320d450692be3a45ebde1b30a2c590d212eed
Author:      dazesace
Email:       dazemcblaze@gmail.com
Date:        2022-12-09T16:11:27Z
Fingerprint: c4b320d450692be3a45ebde1b30a2c590d212eed:pages/measurements/[id].js:generic-api-key:43

Finding:     const token = "VFvJgQzXkq0TVb-M4mLMPGHkw0oXv8XASrfGKfZk9j_5rIrp-Lak20Tdb6zTOJPTGuHhLg6AiWsZkyOQcbrnoQ=="
Secret:      VFvJgQzXkq0TVb-M4mLMPGHkw0oXv8XASrfGKfZk9j_5rIrp-Lak20Tdb6zTOJPTGuHhLg6AiWsZkyOQcbrnoQ==
RuleID:      generic-api-key
Entropy:     5.457852
File:        pages/measurements/[id].js
Line:        49
Commit:      c4b320d450692be3a45ebde1b30a2c590d212eed
Author:      dazesace
Email:       dazemcblaze@gmail.com
Date:        2022-12-09T16:11:27Z
Fingerprint: c4b320d450692be3a45ebde1b30a2c590d212eed:pages/measurements/[id].js:generic-api-key:49

Finding:     ...EXT_PUBLIC_INFLUXDB_TOKEN=m8ghziB2_7h8hmTQfITpWKp-HNvut62ZqfoKmXf8m09DzwiryCWv9VK0E2F48sLn-vGMG6ICpumPISTdXhz_Wg==
NEXT_PUBLIC_INFLUXD...
Secret:      m8ghziB2_7h8hmTQfITpWKp-HNvut62ZqfoKmXf8m09DzwiryCWv9VK0E2F48sLn-vGMG6ICpumPISTdXhz_Wg==
RuleID:      generic-api-key
Entropy:     5.386663
File:        .env
Line:        1
Commit:      321e9ef7adfaf00e78b133a6d7ef94cff9e3c99c
Author:      dazesace
Email:       dazemcblaze@gmail.com
Date:        2022-11-30T15:25:16Z
Fingerprint: 321e9ef7adfaf00e78b133a6d7ef94cff9e3c99c:.env:generic-api-key:1

Finding:     apiKey: "AIzaSyBxyfKVNCT1BG7pn2r9sHvyX4KhKaMgqwU"
Secret:      AIzaSyBxyfKVNCT1BG7pn2r9sHvyX4KhKaMgqwU
RuleID:      gcp-api-key
Entropy:     4.938998
File:        pages/measurements/[id].js
Line:        15
Commit:      7dd3cfb60926825152bed3d2614ad5e3798db071
Author:      dazesace
Email:       dazemcblaze@gmail.com
Date:        2022-11-29T21:15:54Z
Fingerprint: 7dd3cfb60926825152bed3d2614ad5e3798db071:pages/measurements/[id].js:gcp-api-key:15

#### eversion-technologie-landingpage

Finding:     storefrontAccessToken: 'd46ded6b44a81cc29c797a9e6787d1ff'
Secret:      d46ded6b44a81cc29c797a9e6787d1ff
RuleID:      generic-api-key
Entropy:     3.593139
File:        components/Schmerzpatient/shopify-script.js
Line:        23
Commit:      44551241fb59a73ab5ddbdf8acc3c04a35d80a6a
Author:      EVERSION Technologies
Email:       lucas@eversion.tech
Date:        2024-01-24T12:00:31Z
Fingerprint: 44551241fb59a73ab5ddbdf8acc3c04a35d80a6a:components/Schmerzpatient/shopify-script.js:generic-api-key:23

![[Pasted image 20240319112817.png]]

#### eversion_measurement

Finding:     "current_key": "AIzaSyDiUQ6lsAzH3lN9Zj7kBIgfhwzuWM2YSsw"
Secret:      AIzaSyDiUQ6lsAzH3lN9Zj7kBIgfhwzuWM2YSsw
RuleID:      gcp-api-key
Entropy:     4.855790
File:        android/app/google-services.json
Line:        31
Commit:      ebec8179baf822a81ea2835e3e1f1763bba8c742
Author:      dazesace
Email:       dazemcblaze@gmail.com
Date:        2022-10-26T11:36:46Z
Fingerprint: ebec8179baf822a81ea2835e3e1f1763bba8c742:android/app/google-services.json:gcp-api-key:31

![[Pasted image 20240319143631.png]]

Finding:     apiKey: "AIzaSyBxyfKVNCT1BG7pn2r9sHvyX4KhKaMgqwU"
Secret:      AIzaSyBxyfKVNCT1BG7pn2r9sHvyX4KhKaMgqwU
RuleID:      gcp-api-key
Entropy:     4.938998
File:        web/index.html
Line:        67
Commit:      ebec8179baf822a81ea2835e3e1f1763bba8c742
Author:      dazesace
Email:       dazemcblaze@gmail.com
Date:        2022-10-26T11:36:46Z
Fingerprint: ebec8179baf822a81ea2835e3e1f1763bba8c742:web/index.html:gcp-api-key:67

![[Pasted image 20240319144134.png]]

#### eversion_xiao_nrf52840_sense

59 commits scanned.
no leaks found.

#### eversionSensor

5 commits scanned.
no leaks found.
### Countermeasures (only for reference purpose)
- Firebase API keys are generally not considered as secrets, but there might be situations where we need additional protection: https://firebase.google.com/docs/projects/api-keys
- Completely remove sensitive data from the Github repository ('s history): https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository
- Use Github Actions secrets
- Use Codespaces secrets