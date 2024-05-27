Drozer is a penetration testing tool for Android applications. With Drozer, pentesters can interact with Android apps to exploit potentially vulnerable exported activities, content providers, and services.

Install and set up the Console on PC and the Agent on Genymotion: https://github.com/WithSecureLabs/drozer?tab=readme-ov-file

## Retrieve package name:
```
dz> run app.package.list -f key_word
```

Example:
![[Pasted image 20240513193519.png]]

## Identify attack surface:
```
dz> run app.package.attacksurface package_name
```

Example:
![[Pasted image 20240513193748.png]]

## Find exported activities:
```
run app.activity.info -a package_name
```

Example:
![[Pasted image 20240513194031.png]]

## Lauch activities:
```
run app.activity.start --component package_name activity_name
```

Example:
![[Pasted image 20240513195219.png]]

## Show information of Content Providers:
```
run app.provider.info -a package_name
```

Example:
![[Pasted image 20240513200650.png]]

## Scan provider URIs:
```
run scanner.provider.finduris -a package_name
```

Example:
![[Pasted image 20240513200816.png]]

## Retrieve information from content URIs:
```
run app.provider.query URI --vertical
```

Example:
![[Pasted image 20240513201047.png]]

## Check for SQL injection:

We can test for SQL injection by manipulating the projection and selection fields that are passed to the content provider. Examples:
![[Pasted image 20240513201936.png]]
![[Pasted image 20240513201951.png]]

Alternative:
```
run scanner.provider.injection -a package_name
```

Example:
![[Pasted image 20240513203058.png]]

## Read/Download files using file system content providers:
![[Pasted image 20240513202755.png]]
![[Pasted image 20240513202807.png]]

## Find exported services:
```
run app.service.info -a package_name
```

Example:
![[Pasted image 20240513203242.png]]
