# microG Extended Access Bypass for Android TV
This is a custom build of microG GmsCore patched to resolve the fatal SecurityException that prevents official Google TV components from booting in a microG environment.

### The Issue
Official Android TV UI apps rely on shared Google libraries (like OneGoogle) to fetch account data over Binder IPC. However, they do not declare GET_ACCOUNTS in their manifests. By default, microG's internal AuthManagerServiceImpl blocks this undocumented access, resulting in an immediate crash and a persistent black screen/bootloop on Android TV devices.

### The Fix
This release modifies PackageUtils.java (callerHasGooglePackagePermission) to bypass the strict cryptographic signature check. It silently grants "Extended Access" to specific proprietary TV UI components, allowing them to initialize alongside microG.

### Whitelisted Packages
The following apps will now successfully bypass the Extended Access check:

> com.google.android.tvlauncher

> com.google.android.tvrecommendations

> com.google.android.apps.tv.launcherx

### Resolves
```
com.google.android.gms
java.lang.SecurityException: Access denied, missing google package permission for ACCOUNT
```
