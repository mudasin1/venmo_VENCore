---
title: NSDictionary+VENCore Category Implementation
---
# Introduction

This document explains key design choices in the <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="1:4:4" line-data="#import &quot;NSDictionary+VENCore.h&quot;">`NSDictionary`</SwmToken>+<SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="1:6:6" line-data="#import &quot;NSDictionary+VENCore.h&quot;">`VENCore`</SwmToken> category implementation. It focuses on how the category extends <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="1:4:4" line-data="#import &quot;NSDictionary+VENCore.h&quot;">`NSDictionary`</SwmToken> and <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="4:3:3" line-data="@implementation NSMutableDictionary (VENCore)">`NSMutableDictionary`</SwmToken> to handle common data cleaning and type-safe access patterns.

We will cover:

1. How null values and nested collections are cleansed from dictionaries.
2. How safe retrieval of values is implemented to avoid <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="9:9:9" line-data="        if (object == [NSNull null]) {">`NSNull`</SwmToken> issues.
3. How type conversions are handled when accessing dictionary values.

# cleansing dictionaries and nested collections

The <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="4:3:3" line-data="@implementation NSMutableDictionary (VENCore)">`NSMutableDictionary`</SwmToken> category adds a method to remove <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="9:9:9" line-data="        if (object == [NSNull null]) {">`NSNull`</SwmToken> values and convert nested collections recursively. This is important because JSON responses often contain <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="9:9:9" line-data="        if (object == [NSNull null]) {">`NSNull`</SwmToken> placeholders which cause crashes or require extra checks downstream.

The method iterates over all keys and:

- Removes keys with <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="9:9:9" line-data="        if (object == [NSNull null]) {">`NSNull`</SwmToken> values.
- Converts <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="12:12:12" line-data="        else if ([object isKindOfClass:[NSNumber class]]) {">`NSNumber`</SwmToken> values to <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="7:4:4" line-data="    for (NSString *key in [self allKeys]) {">`NSString`</SwmToken> for consistency.
- Recursively cleans nested <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="1:4:4" line-data="#import &quot;NSDictionary+VENCore.h&quot;">`NSDictionary`</SwmToken> and <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="2:4:4" line-data="#import &quot;NSArray+VENCore.h&quot;">`NSArray`</SwmToken> objects by calling their cleansing methods.

<SwmSnippet path="/VENCore/Categories/NSDictionary+VENCore.m" line="1">

---

This approach centralizes data normalization right after receiving a response, so the rest of the code can work with clean, predictable data structures without nulls or mixed types.

```limbo
#import "NSDictionary+VENCore.h"
#import "NSArray+VENCore.h"

@implementation NSMutableDictionary (VENCore)

- (void)cleanseResponseDictionary {
    for (NSString *key in [self allKeys]) {
        NSObject *object = (NSObject *) self[key];
        if (object == [NSNull null]) {
            [self removeObjectForKey:key];
        }
        else if ([object isKindOfClass:[NSNumber class]]) {
            self[key] = [((NSNumber *)object) stringValue];
        }
        else if ([object isKindOfClass:[NSDictionary class]]) {
            self[key] = [((NSDictionary *)object) dictionaryByCleansingResponseDictionary];
        }
        else if([object isKindOfClass:[NSArray class]]) {
            NSArray *array = [(NSArray *)object copy];
            self[key] = [array arrayByCleansingResponseArray];
        }
    }
}
```

---

</SwmSnippet>

# safe value retrieval avoiding <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="9:9:9" line-data="        if (object == [NSNull null]) {">`NSNull`</SwmToken>

The <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="1:4:4" line-data="#import &quot;NSDictionary+VENCore.h&quot;">`NSDictionary`</SwmToken> category adds a method to safely get values for keys, returning nil instead of <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="9:9:9" line-data="        if (object == [NSNull null]) {">`NSNull`</SwmToken>. This prevents the common bug where <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="9:9:9" line-data="        if (object == [NSNull null]) {">`NSNull`</SwmToken> is returned and causes crashes or unexpected behavior when used directly.

<SwmSnippet path="/VENCore/Categories/NSDictionary+VENCore.m" line="25">

---

This method is a simple but effective guard that should be used whenever accessing dictionary values that might come from external sources.

```limbo
@end

@implementation NSDictionary (VENCore)

- (id)objectOrNilForKey:(id)key {
    id object = self[key];
    return object == [NSNull null] ? nil : object;
}
```

---

</SwmSnippet>

# type-safe accessors for bool and string

To avoid repetitive type checks and conversions, the category provides convenience methods for retrieving boolean and string values.

- The bool accessor returns the boolean value if the object responds to <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="37:11:11" line-data="    if ([object respondsToSelector:@selector(boolValue)]) {">`boolValue`</SwmToken>, otherwise it returns YES if the object exists (non-nil).
- The string accessor returns the <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="13:18:18" line-data="            self[key] = [((NSNumber *)object) stringValue];">`stringValue`</SwmToken> if available, otherwise returns the object itself.

<SwmSnippet path="/VENCore/Categories/NSDictionary+VENCore.m" line="35">

---

These methods reduce boilerplate and make the calling code cleaner by encapsulating common type coercion logic.

```limbo
- (BOOL)boolForKey:(id)key {
    id object = [self objectOrNilForKey:key];
    if ([object respondsToSelector:@selector(boolValue)]) {
        return [object boolValue];
    } else {
        return object != nil;
    }
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Categories/NSDictionary+VENCore.m" line="45">

---

&nbsp;

```limbo
- (NSString *)stringForKey:(id)key {
    id object = [self objectOrNilForKey:key];
    return [object respondsToSelector:@selector(stringValue)] ? [object stringValue] : object;
}


- (instancetype)dictionaryByCleansingResponseDictionary {
```

---

</SwmSnippet>

# creating cleansed immutable dictionaries

Since cleansing modifies the dictionary, an immutable <SwmToken path="VENCore/Categories/NSDictionary+VENCore.m" pos="1:4:4" line-data="#import &quot;NSDictionary+VENCore.h&quot;">`NSDictionary`</SwmToken> method is provided that returns a cleansed copy. It creates a mutable copy, runs the cleansing, then returns an immutable dictionary.

<SwmSnippet path="/VENCore/Categories/NSDictionary+VENCore.m" line="53">

---

This preserves immutability guarantees while still allowing the cleansing logic to be reused.

```limbo
    NSMutableDictionary *dictionary  = [self mutableCopy];
    [dictionary cleanseResponseDictionary];

    return [NSDictionary dictionaryWithDictionary:dictionary];
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBdmVubW9fVkVOQ29yZSUzQSUzQW11ZGFzaW4x" repo-name="venmo_VENCore"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
