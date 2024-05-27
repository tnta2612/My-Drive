


# Bluetooth LE Sensor Static Passkey

The sensors can be connected to the mobile app via Bluetooth LE given a static passkey.

## Finding:

The static nature of the 6-digit passkey poses a significant security risk. Once intercepted or compromised, an attacker could gain unauthorized access to the sensor network. Additionally, since the passkey remains unchanged, it provides a prolonged window of opportunity for exploitation.

## Criticality:

Informational
CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:N/VI:N/VA:N/SC:N/SI:N/SA:N

## Countermeasures:

1. Enable users to change the passkeys of their sensors periodically or on-demand. This feature enhances security by mitigating the risk associated with a compromised passkey. Users should be encouraged to use strong, unique passphrases that are resistant to common attack methods.



# Bluetooth LE Sensor Limited Character Set

The sensors can be connected to the mobile app via Bluetooth LE given a static passkey.

## Finding:

Restricting the passkey to only digits limits the complexity and strength of the authentication mechanism. Attackers can exploit this limitation by employing brute-force attacks, reducing the time required to guess the passkey.

## Criticality:

Informational
CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:N/VI:N/VA:N/SC:N/SI:N/SA:N

## Countermeasures:

1. Passkeys should be of sufficient length and complexity, incorporating a mix of uppercase letters, lowercase letters, digits, and special characters. This enhances security by introducing unpredictability and making brute-force attacks significantly more challenging.

 
 
# Method Confusion Attack on Bluetooth Pairing

In the Method Confusion Attack, the attacker positions themselves as a Man-in-the-Middle (MitM) between two Bluetooth devices, intercepting and manipulating their communication. By coercing the devices into using different pairing methods, the attacker exploits vulnerabilities in the Bluetooth protocol stack to gain unauthorized access to sensitive data exchanged between the devices.

## Finding:

The Bluetooth pairing process between the mobile application and the sensors is susceptible to the Method Confusion Attack.

## Criticality:

7.6 / High
CVSS:4.0/AV:N/AC:L/AT:P/PR:N/UI:P/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N

## Countermeasures:

The Method Confusion Attack remains relevant today due to its effectiveness in exploiting inherent weaknesses in Bluetooth pairing protocols. Mitigating this attack requires fundamental changes to the Bluetooth pairing protocol, a process that may take considerable time and resources. As such, Eversion Tech may need to acknowledge and accept the risk posed by this attack, especially because modifying the pairing protocol is beyond the scope of immediate implementation.
