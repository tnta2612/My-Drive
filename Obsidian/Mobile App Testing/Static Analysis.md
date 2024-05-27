


## Check if backups are enabled:

Backups enabled are considered a security issue because people could back up your app via ADB and then get private data of your app into their PC.

```
cat AndroidManifest.xml | grep "android:allowBackup" --color
```

![[Pasted image 20240512210411.png]]



## Check if debugging is enabled:

Debugging enabled allows dumping a stack trace and accessing debugging helper classes.

```
cat AndroidManifest.xml | grep "android:debuggable" --color
```



## Check for unreasonable dangerous app permissions:

Dangerous permissions can give apps access to things like calling history, private messages, location, camera, microphone, and more.

```
cat AndroidManifest.xml | grep "android.permission" --color
```



## Scan for FireBase misconfigurations:

> [!info]
> Use FireBaseScanner to scan for FireBase misconfigurations: https://github.com/shivsahni/FireBaseScanner

```
python FirebaseMisconfig.py -p /path/to/apk
```



## Insecure data storage

Each application possesses a default data directory located at /data/data/<package_name>. This directory serves as the default storage location for the app's databases, settings, and other essential data. Here's a breakdown of its contents:

- **databases/**: Stores the application's databases.
- **lib/**: Contains libraries and helpers utilized by the app.
- **files/**: Houses additional relevant files.
- **shared_prefs/**: Contains preferences and settings.
- **cache/**: Stores cached data.

Once we gain access to the database files, we can proceed to inspect them:

### Using adb shell:

```
adb shell
```

Then, we can go to the database files and inspect them. For example:
![[Pasted image 20240514094041.png]]

### Using a GUI software

1. Pull  database files from the device:  
```
adb pull /data/data/package-name/databases/database-name
```

2. Use a GUI software to inspect the database. For example:

![[Pasted image 20240514094345.png]]



## Shared Preferences Files

The SharedPreferences API is frequently utilized for persistently storing small collections of key-value pairs. Data saved within a SharedPreferences object is typically written to plain-text XML files. These files can be configured as either world-readable (accessible to all apps) or private. It's crucial to examine these files to determine if any sensitive data is inadvertently exposed.

Example: