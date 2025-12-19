---
title: UIDevice VENCore Category Implementation
---
# Introduction

This document explains the key design choices behind the <SwmToken path="VENCore/Categories/UIDevice+VENCore.m" pos="1:4:4" line-data="#import &quot;UIDevice+VENCore.h&quot;">`UIDevice`</SwmToken> category in <SwmToken path="VENCore/Categories/UIDevice+VENCore.m" pos="1:6:6" line-data="#import &quot;UIDevice+VENCore.h&quot;">`VENCore`</SwmToken>. It answers:

1. How does the code retrieve the device platform identifier?
2. How does it generate and persist a unique device ID?

# getting the device platform string

The method to get the platform string uses low-level system calls (<SwmToken path="VENCore/Categories/UIDevice+VENCore.m" pos="11:1:1" line-data="    sysctlbyname(&quot;hw.machine&quot;, NULL, &amp;size, NULL, 0);">`sysctlbyname`</SwmToken>) to query the hardware machine name. This returns identifiers like "iPhone10,1" which specify the exact device model. The code allocates a buffer dynamically based on the required size, fetches the string, converts it to an <SwmToken path="VENCore/Categories/UIDevice+VENCore.m" pos="9:3:3" line-data="- (NSString *)VEN_platformString {">`NSString`</SwmToken>, and then frees the buffer.

<SwmSnippet path="/VENCore/Categories/UIDevice+VENCore.m" line="9">

---

This approach is preferred because it directly queries the system for the hardware identifier rather than relying on higher-level APIs that might abstract or mask this detail. It’s useful for analytics, debugging, or device-specific behavior.

```limbo
- (NSString *)VEN_platformString {
    size_t size;
    sysctlbyname("hw.machine", NULL, &size, NULL, 0);
    char *machine = malloc(size);
    sysctlbyname("hw.machine", machine, &size, NULL, 0);
    NSString *platform = [NSString stringWithCString:machine encoding:NSUTF8StringEncoding];
    free(machine);
    return platform;
}
```

---

</SwmSnippet>

# generating and storing a unique device ID

The unique device ID method first checks if a UUID is already stored in <SwmToken path="VENCore/Categories/UIDevice+VENCore.m" pos="21:1:1" line-data="    NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];">`NSUserDefaults`</SwmToken> under a specific key. If not, it creates a new UUID using Core Foundation’s <SwmToken path="VENCore/Categories/UIDevice+VENCore.m" pos="24:7:7" line-data="        CFUUIDRef uuid = CFUUIDCreate(kCFAllocatorDefault);">`CFUUIDCreate`</SwmToken>, converts it to a string, stores it in <SwmToken path="VENCore/Categories/UIDevice+VENCore.m" pos="21:1:1" line-data="    NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];">`NSUserDefaults`</SwmToken>, and synchronizes immediately.

<SwmSnippet path="/VENCore/Categories/UIDevice+VENCore.m" line="20">

---

This ensures the app has a consistent unique identifier per device without relying on deprecated or privacy-sensitive APIs. Storing it in <SwmToken path="VENCore/Categories/UIDevice+VENCore.m" pos="21:1:1" line-data="    NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];">`NSUserDefaults`</SwmToken> means the ID persists across app launches but resets if the app is deleted.

```limbo
- (NSString *)VEN_deviceIDString {
    NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];
    NSString *uniqueIdentifier = [userDefaults stringForKey:VENUserDefaultsKeyDeviceID];
    if (!uniqueIdentifier) {
        CFUUIDRef uuid = CFUUIDCreate(kCFAllocatorDefault);
        uniqueIdentifier = CFBridgingRelease(CFUUIDCreateString(kCFAllocatorDefault, uuid));
        CFRelease(uuid);
        [userDefaults setObject:uniqueIdentifier forKey:VENUserDefaultsKeyDeviceID];
        [userDefaults synchronize];
    }
    return uniqueIdentifier;
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBdmVubW9fVkVOQ29yZSUzQSUzQW11ZGFzaW4x" repo-name="venmo_VENCore"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
