
Okay ich habe mir das Raum Android Hacking 101 komplett angeschaut. Es geht ja um Mobile Application Penetration Testing. Heute würde ich euch gerne zeigen, was man da machen könnte.

Okay, erstmal müssten wir ja die Testumgebung einrichten. Dazu brauchen wie entweder ein gerooted Smmartphone, oder einen Emulator. Für Emulatoren gibt es die GenyMotion oder NoxEmulator. Beide kann mit homebrew installiert werden. Allerdings konnte ich den NoxxEmulator auf meinem Macbook nicht öffnen. Es wurde gemeint, dass Apple diese App nicht prüfen konnte und ich konnte damit den NoxEmulator nicht öffnen. Ich habe keine Ahnung, wir man die Prüfung von Apple umgehen könnte. Deswegen haben wir nur noch die Möglichkeit, mit GenyMotion zu arbeiten. 

Es sieht so aus. Da kann man verschiedene Android-Emulatoren anlegen. Allerdings ist der Emulator ziemlich umständlich. Ich konnte keine App installieren, weil es kein Google Play auf dem Emulator gibt, weil es ja gerooted ist. Dann habe ich versucht, eine APK Datei aus dem Internet herunterzuladen. Das ist mir leider auch nicht gelungen. Deswegen ist mir nur noch die letzte Möglichkeit geblieben, mit meinem eigenen Android-Smartphone zu arbeiten. Mein Smartphone ist ja nicht rooted. Und ich wollte das auch nicht machen, weil damit die Garantie abläuft. Deswegen habe ich weiter gearbeitet, ohne mein Smartphone zu rooten.

Erstmal müssten wir die Developer-Mode aktivieren, und danach USB-Debugging aktivieren. Dann müssten wir noch eine App aus Google Play zum Testen aussuchen. Ich habe den Hacker Tracker von defcon benutzt.

Man kann verschiedene Tools zum Testen verwenden z.B:
- adb: das braucht man unbedingt. Ich habe später noch festgestellt, dass man mit adb Apps in dem Emulator noch installieren kann. Dafür braucht man ja noch die APK Datei.
- jadx: zum Extrahieren des Java SourceCode aus dem APK Datei
- dex2jar: zum Umwandeln von einer APK-Datei auf eine JAR-Datei
- JD-GUI: zum Öffnen dieser JAR-Datei zum Lesen des JAVA-Code. Aber das könnte ich auf meinem Macbook nicht öffnen, weil Apple das nicht erlaubt.
- apktool: zum Extrahieren des smali SourceCode. Smali ist eine Assembly-Sprache, die auf Adroid's JVM läuft.
- Und so weiter

Okay jetzt können wir mit der Static Analysis anfangen

Die erste interessante Datei, die wir prüfen könnten, ist die AndroidManifest.xml. Diese Datei spezifiziert verschiedene Metadaten, under anderem sind die vergebenen Rechte.

Erstmal können wir prüfen, ob Backup zugelassen ist. Dies ist sicherheitskritisch, weil mit Backup kann man ja alle privaten Daten aus der App in den PC kopieren.
Dann kann man noch prüfen, ob Debug zugelassen ist. Dies ist auch sicherheitskritisch, weil mit Debug kann ein Angreifer ja weitere nützliche Informationen zum Reverse Engineering gewinnen.

Dann können wir mit dieser AndroidManifest Datei noch prüfen, welche Permissions die App verlangt. Da könnten wir ja feststellen, ob die App unnötige Persmissions verlangt.

Dann haben wir den Firebase Scanner. Firebase ist eine der beliebtesten Datenbanken von Google für Android und iOS Apps. Deswegen macht es schon Sinn, auf die Miskonfigurationen von diesen Datenbanken zu prüfen. Und genau das macht der Firebase Scanner. Ich habe keine Miskonfiguration gefunden. Deswegen bin ich mir nicht sicher, wie gut dieses Zeug ist. Es ist aber ziemlich alt und wurde in Python 2 geschrieben.

Danach haben wir Tools zum Static Analysis. Erstmal haben wir das MARA Framework. Dieses Framework enthält viele kleinere Tools zum Reverse Engineering und Analysis. Das Framework ist aber nicht so beliebt. Der letzte Beitrag auf Github war im 2015. Deswegen gehe ich davon aus, dass dieses Framework nicht das bester ist. 

Danach haben wir Qark, auch ein Static Code Analysis Tool, wie SonarQube. Der ist beliebter and und besser als das MARA Framework. Ich habe Qark auch in Detail ausprobiert. Der hat leider zu viele False Positives geliefert.

Zuletzt haben wir noch MobSF, der beliebteste von allen drei. Ich glaube, es ist auch der beste von allen drei. Mit MobSF kann man Static Analysis machen. Man muss dem MobSF nur die APK Datei übergeben. dann bekommt man eine Analysis zurück, mit Findings und Ratings. Da kann man die Findings durchlaufen und analysieren. Also, das Tool ist ziemlich straigtforward und umfassend.

Neben dem Static Analysis kann man mit MobSF auch Dynamic Analysis machen. Da kann man verschiedene Dinge machen, z.B. Traffic über BurpSuite weiterleiten, und Burp als Proxy verwenden. Ich bin aber bei dem Installieren von BurpSuite-Zertifikat leider nicht weitergekommen. Der Prozess ist ziemlich kompliziert und unterschiedlich auf verschiedenen Smartphones. Und um das machen zu können, muss das Handy auch gerooted werden. Mein Handy ist ja nicht gerooted. Und ich bin letzte Woche dabei leider nicht weitergekommen. Ich würde diese Woche noch kurz anschauen und vielleicht Umweg finden. Und ja, mein Vortrag kommt damit zu Ende, nächste Woche könnte ich euch vielleicht noch etwas über Dynamic Analysis erzählen.

Und vielleicht noch ein Wort zu diesem Raum auf tryhackme. Der ist eigentlich für das Tooling. Da werden verschiedene Tools zum Testen vorstellt, aber nicht in Detail. Da müsste man selber ausprobieren. Und da lernt man auch fast keine Mobile Security Concepts, sondern nur das Tooling.

Ja, damit bin ich fertig. Habt ich Fragen?


1. Check if the daemon is running and start it: adb devices
2. Print the path to the APK: adb shell pm path package_name
3. To extract the APK: adb pull <remote_path> [<locaDestination_path>]
4. Get the source code: jadx -d [path-output-folder] [path-to-apk-or-dex-file]
5. Get JAR from APK: d2j-dex2jar /path/application.apk
6. Get source code in smalli: apktool d file.apk
7. Get the activities: cat AndroidManifest.xml | grep "activity" --color
8. Check if backup allowed: cat AndroidManifest.xml | grep "android:allowBackup" --color
9. Check if debug allowed: cat AndroidManifest.xml | grep "android:debuggable" --color
10. Check uses-permission: cat AndroidManifest.xml | grep "uses-permission" --color
11. Use Firebase Scanner in Home directory: python3 FirebaseMisconfig.py -p ~/Downloads/base.apk




