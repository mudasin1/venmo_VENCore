---
title: NSString Category for Validation Methods
---
# Introduction

This document explains the rationale behind adding validation methods as a category on <SwmToken path="VENCore/Categories/NSString+VENCore.m" pos="1:4:4" line-data="#import &quot;NSString+VENCore.h&quot;">`NSString`</SwmToken>. It covers:

1. Why validation methods are implemented as <SwmToken path="VENCore/Categories/NSString+VENCore.m" pos="1:4:4" line-data="#import &quot;NSString+VENCore.h&quot;">`NSString`</SwmToken> category methods.
2. How phone number, email, and user ID validations are distinguished.
3. The purpose of the <SwmToken path="VENCore/Categories/NSString+VENCore.m" pos="27:5:5" line-data="- (VENTargetType)targetType {">`targetType`</SwmToken> method for identifying the kind of string.
4. The role of the <SwmToken path="VENCore/Categories/NSString+VENCore.m" pos="43:5:5" line-data="- (BOOL)hasContent {">`hasContent`</SwmToken> method in checking string emptiness beyond whitespace.

# why use an <SwmToken path="VENCore/Categories/NSString+VENCore.m" pos="1:4:4" line-data="#import &quot;NSString+VENCore.h&quot;">`NSString`</SwmToken> category for validation

Adding validation methods directly to <SwmToken path="VENCore/Categories/NSString+VENCore.m" pos="1:4:4" line-data="#import &quot;NSString+VENCore.h&quot;">`NSString`</SwmToken> via a category keeps validation logic close to the string data it operates on. This avoids scattering validation code across the app and makes the validations reusable and easy to call on any <SwmToken path="VENCore/Categories/NSString+VENCore.m" pos="1:4:4" line-data="#import &quot;NSString+VENCore.h&quot;">`NSString`</SwmToken> instance. It also leverages Objective-C’s dynamic nature to extend <SwmToken path="VENCore/Categories/NSString+VENCore.m" pos="1:4:4" line-data="#import &quot;NSString+VENCore.h&quot;">`NSString`</SwmToken> without subclassing.

# how phone, email, and user ID validations work

The <SwmToken path="VENCore/Categories/NSString+VENCore.m" pos="5:5:5" line-data="- (BOOL)isUSPhone {">`isUSPhone`</SwmToken> method uses a regular expression to match US phone number formats, including optional country code and separators. This regex is strict enough to catch common valid US phone numbers.

The <SwmToken path="VENCore/Categories/NSString+VENCore.m" pos="12:5:5" line-data="- (BOOL)isEmail {">`isEmail`</SwmToken> method also uses a regex pattern to validate email addresses. It first lowercases the string to simplify matching, then applies a pattern that covers standard email formats.

The <SwmToken path="VENCore/Categories/NSString+VENCore.m" pos="20:5:5" line-data="- (BOOL)isUserId {">`isUserId`</SwmToken> method checks if the string consists only of digits but excludes strings that qualify as US phone numbers. This ensures user IDs are numeric strings distinct from phone numbers.

<SwmSnippet path="/VENCore/Categories/NSString+VENCore.m" line="1">

---

These methods separate concerns by validating different string types explicitly, which helps downstream logic decide how to handle the string.

```limbo
#import "NSString+VENCore.h"

@implementation NSString (VENCore)

- (BOOL)isUSPhone {
    NSString *phoneRegex = @"^\\+?1?\\D{0,2}(\\d{3})\\D{0,2}\\D?(\\d{3})\\D?(\\d{4})$";
    NSPredicate *phoneTest = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", phoneRegex];
    return [phoneTest evaluateWithObject:self];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Categories/NSString+VENCore.m" line="12">

---

&nbsp;

```limbo
- (BOOL)isEmail {
    NSString *lowerCaseSelf = [self lowercaseString];
    NSString *pattern = @"[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?";
    NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", pattern];
    return [predicate evaluateWithObject:lowerCaseSelf];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Categories/NSString+VENCore.m" line="20">

---

&nbsp;

```limbo
- (BOOL)isUserId {
    NSCharacterSet *notDigitSet = [[NSCharacterSet characterSetWithCharactersInString:@"0123456789"] invertedSet];
    return ![self isUSPhone] &&
            [self rangeOfCharacterFromSet:notDigitSet].location == NSNotFound;
}


- (VENTargetType)targetType {
    if ([self isUSPhone]) {
        return VENTargetTypePhone;
    }
    else if ([self isEmail]) {
        return VENTargetTypeEmail;
    }
    else if ([self isUserId]) {
        return VENTargetTypeUserId;
    }
    else {
        return VENTargetTypeUnknown;
    }
}
```

---

</SwmSnippet>

# how <SwmToken path="VENCore/Categories/NSString+VENCore.m" pos="27:5:5" line-data="- (VENTargetType)targetType {">`targetType`</SwmToken> identifies string kind

The <SwmToken path="VENCore/Categories/NSString+VENCore.m" pos="27:5:5" line-data="- (VENTargetType)targetType {">`targetType`</SwmToken> method uses the above validation methods in order to classify the string as a phone number, email, user ID, or unknown. This centralizes the logic for determining the string’s semantic type, which can be used elsewhere for routing or processing.

<SwmSnippet path="/VENCore/Categories/NSString+VENCore.m" line="20">

---

By checking phone first, then email, then user ID, it prioritizes more specific formats before falling back to numeric user IDs.

```limbo
- (BOOL)isUserId {
    NSCharacterSet *notDigitSet = [[NSCharacterSet characterSetWithCharactersInString:@"0123456789"] invertedSet];
    return ![self isUSPhone] &&
            [self rangeOfCharacterFromSet:notDigitSet].location == NSNotFound;
}


- (VENTargetType)targetType {
    if ([self isUSPhone]) {
        return VENTargetTypePhone;
    }
    else if ([self isEmail]) {
        return VENTargetTypeEmail;
    }
    else if ([self isUserId]) {
        return VENTargetTypeUserId;
    }
    else {
        return VENTargetTypeUnknown;
    }
}
```

---

</SwmSnippet>

# checking if a string has meaningful content

The <SwmToken path="VENCore/Categories/NSString+VENCore.m" pos="43:5:5" line-data="- (BOOL)hasContent {">`hasContent`</SwmToken> method trims whitespace and checks if anything remains. This is useful to quickly reject empty or whitespace-only strings before attempting validation or processing.

<SwmSnippet path="/VENCore/Categories/NSString+VENCore.m" line="43">

---

It’s a lightweight guard to avoid unnecessary work on empty inputs.

```limbo
- (BOOL)hasContent {
    NSCharacterSet *set = [NSCharacterSet whitespaceCharacterSet];
    if ([[self stringByTrimmingCharactersInSet: set] length] == 0)
    {
        return NO;
    }
    return YES;
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBdmVubW9fVkVOQ29yZSUzQSUzQW11ZGFzaW4x" repo-name="venmo_VENCore"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
