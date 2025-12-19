---
title: NSArray+VENCore Category Implementation
---
# Introduction

This document explains the rationale behind the <SwmToken path="VENCore/Categories/NSArray+VENCore.m" pos="1:4:4" line-data="#import &quot;NSArray+VENCore.h&quot;">`NSArray`</SwmToken>+<SwmToken path="VENCore/Categories/NSArray+VENCore.m" pos="1:6:6" line-data="#import &quot;NSArray+VENCore.h&quot;">`VENCore`</SwmToken> category implementation. It focuses on:

1. Why we need to cleanse arrays in the response data.
2. How the cleansing process handles different data types inside arrays.
3. The design choice of providing both mutable and immutable array cleansing methods.

# why cleanse response arrays

The main goal is to prepare arrays received from external sources (like APIs) for safer and more consistent use within the app. Raw response data can contain <SwmToken path="VENCore/Categories/NSArray+VENCore.m" pos="13:14:14" line-data="        else if (self[i] == [NSNull null]) {">`NSNull`</SwmToken> objects, <SwmToken path="VENCore/Categories/NSArray+VENCore.m" pos="10:13:13" line-data="        if ([self[i] isKindOfClass:[NSNumber class]]) {">`NSNumber`</SwmToken> instances where strings are expected, or nested dictionaries and arrays that also need cleansing. This category ensures the data is normalized by:

- Converting <SwmToken path="VENCore/Categories/NSArray+VENCore.m" pos="10:13:13" line-data="        if ([self[i] isKindOfClass:[NSNumber class]]) {">`NSNumber`</SwmToken> objects to NSString.
- Removing <SwmToken path="VENCore/Categories/NSArray+VENCore.m" pos="13:14:14" line-data="        else if (self[i] == [NSNull null]) {">`NSNull`</SwmToken> objects entirely.
- Recursively cleansing nested dictionaries and arrays.

This prevents the rest of the app from having to handle these edge cases repeatedly.

# how cleansing works for mutable arrays

The method <SwmToken path="VENCore/Categories/NSArray+VENCore.m" pos="6:5:5" line-data="- (void)cleanseResponseArray">`cleanseResponseArray`</SwmToken> operates directly on <SwmToken path="VENCore/Categories/NSArray+VENCore.m" pos="4:3:3" line-data="@implementation NSMutableArray (VENCore)">`NSMutableArray`</SwmToken> instances. It iterates over each element and:

- Converts <SwmToken path="VENCore/Categories/NSArray+VENCore.m" pos="10:13:13" line-data="        if ([self[i] isKindOfClass:[NSNumber class]]) {">`NSNumber`</SwmToken> elements to their string representation.
- Collects <SwmToken path="VENCore/Categories/NSArray+VENCore.m" pos="13:14:14" line-data="        else if (self[i] == [NSNull null]) {">`NSNull`</SwmToken> elements for removal after iteration to avoid mutation during enumeration.
- Recursively calls cleansing methods on nested <SwmToken path="VENCore/Categories/NSArray+VENCore.m" pos="2:4:4" line-data="#import &quot;NSDictionary+VENCore.h&quot;">`NSDictionary`</SwmToken> and <SwmToken path="VENCore/Categories/NSArray+VENCore.m" pos="1:4:4" line-data="#import &quot;NSArray+VENCore.h&quot;">`NSArray`</SwmToken> elements.

<SwmSnippet path="/VENCore/Categories/NSArray+VENCore.m" line="1">

---

This approach modifies the array in place, which is efficient when you already have a mutable array and want to avoid extra copying.

```limbo
#import "NSArray+VENCore.h"
#import "NSDictionary+VENCore.h"

@implementation NSMutableArray (VENCore)

- (void)cleanseResponseArray
{
    NSMutableArray *elementsToRemoveArray = [[NSMutableArray alloc] initWithCapacity:[self count]];
    for (NSUInteger i=0; i<[self count]; i++) {
        if ([self[i] isKindOfClass:[NSNumber class]]) {
            self[i] = [(NSNumber *)self[i] stringValue];
        }
        else if (self[i] == [NSNull null]) {
            [elementsToRemoveArray addObject:self[i]];
        }
        else if ([self[i] isKindOfClass:[NSDictionary class]]) {
            self[i] = [(NSMutableDictionary *)self[i] dictionaryByCleansingResponseDictionary];
        }
        else if ([self[i] isKindOfClass:[NSArray class]]) {
            self[i] = [(NSArray *)self[i] arrayByCleansingResponseArray];
        }
    }
    for (NSObject *objectToRemove in elementsToRemoveArray) {
        [self removeObject:objectToRemove];
    }
}
```

---

</SwmSnippet>

# providing an immutable cleansing method

Since many arrays are immutable by default, the category also offers <SwmToken path="VENCore/Categories/NSArray+VENCore.m" pos="20:19:19" line-data="            self[i] = [(NSArray *)self[i] arrayByCleansingResponseArray];">`arrayByCleansingResponseArray`</SwmToken> on <SwmToken path="VENCore/Categories/NSArray+VENCore.m" pos="1:4:4" line-data="#import &quot;NSArray+VENCore.h&quot;">`NSArray`</SwmToken>. This method:

- Creates a mutable copy of the original array.
- Calls the mutable cleansing method on that copy.
- Returns a new immutable <SwmToken path="VENCore/Categories/NSArray+VENCore.m" pos="1:4:4" line-data="#import &quot;NSArray+VENCore.h&quot;">`NSArray`</SwmToken> with the cleansed content.

<SwmSnippet path="/VENCore/Categories/NSArray+VENCore.m" line="28">

---

This design respects immutability while still enabling cleansing, allowing callers to use the method safely without worrying about side effects on the original array.

```limbo
@end

@implementation NSArray (VENCore)

- (instancetype)arrayByCleansingResponseArray
{
    NSMutableArray *array = [self mutableCopy];
    [array cleanseResponseArray];
    return [NSArray arrayWithArray:array];
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBdmVubW9fVkVOQ29yZSUzQSUzQW11ZGFzaW4x" repo-name="venmo_VENCore"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
