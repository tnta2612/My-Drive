
## View connected devices:

```
adb devices
```

This command also checks if the adb daemon is running and starts it.

![[Pasted image 20240512145448.png]]

## Extract the APK file:

**Prerequisites**: the application is installed on the device and the package name is known.

> [!Tip] Pakage name can be found on Google Play or by using Drozer
> ![[Pasted image 20240512173854.png]]
> 
> 

1. Print the path to the APK: 
```
adb shell pm path package_name
```

![[Pasted image 20240512174119.png]]

2. Extract the APK: 
```
adb pull <remote_path> [<locaDestination_path>]
```

![[Pasted image 20240512174141.png]]

## Get the source code:

```
jadx -d [path-output-folder] [path-to-apk-or-dex-file]
```

This command helps in case the source code is not provided.



