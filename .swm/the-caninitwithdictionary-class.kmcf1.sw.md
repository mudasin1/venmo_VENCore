---
title: 'The canInitWithDictionary: class'
---
This document explains the class method <SwmToken path="VENCore/Models/Users/VENUser.m" pos="73:5:5" line-data="+ (BOOL)canInitWithDictionary:(NSDictionary *)dictionary {">`canInitWithDictionary`</SwmToken>: in <SwmPath>[VENCore/…/Users/VENUser.m](VENCore/Models/Users/VENUser.m)</SwmPath>. We will cover:

1. What <SwmToken path="VENCore/Models/Users/VENUser.m" pos="73:5:5" line-data="+ (BOOL)canInitWithDictionary:(NSDictionary *)dictionary {">`canInitWithDictionary`</SwmToken>: is and its purpose.
2. The variables and functions related to <SwmToken path="VENCore/Models/Users/VENUser.m" pos="73:5:5" line-data="+ (BOOL)canInitWithDictionary:(NSDictionary *)dictionary {">`canInitWithDictionary`</SwmToken>: and their roles.

# What is <SwmToken path="VENCore/Models/Users/VENUser.m" pos="73:5:5" line-data="+ (BOOL)canInitWithDictionary:(NSDictionary *)dictionary {">`canInitWithDictionary`</SwmToken>:

The class method <SwmToken path="VENCore/Models/Users/VENUser.m" pos="73:5:5" line-data="+ (BOOL)canInitWithDictionary:(NSDictionary *)dictionary {">`canInitWithDictionary`</SwmToken>: in <SwmToken path="VENCore/Models/Users/VENUser.m" pos="38:1:1" line-data="    VENUser *newUser        = [[[self class] alloc] init];">`VENUser`</SwmToken> is a validation method that determines whether a given <SwmToken path="VENCore/Models/Users/VENUser.m" pos="8:8:8" line-data="- (instancetype)initWithDictionary:(NSDictionary *)dictionary {">`NSDictionary`</SwmToken> contains sufficient and valid data to initialize a <SwmToken path="VENCore/Models/Users/VENUser.m" pos="38:1:1" line-data="    VENUser *newUser        = [[[self class] alloc] init];">`VENUser`</SwmToken> instance. It checks if the input is a dictionary and verifies the presence and non-empty values of required keys. This method is used to ensure that only dictionaries with the necessary user information are used to create <SwmToken path="VENCore/Models/Users/VENUser.m" pos="38:1:1" line-data="    VENUser *newUser        = [[[self class] alloc] init];">`VENUser`</SwmToken> objects.

<SwmSnippet path="/VENCore/Models/Users/VENUser.m" line="8">

---

The function <SwmToken path="VENCore/Models/Users/VENUser.m" pos="8:5:5" line-data="- (instancetype)initWithDictionary:(NSDictionary *)dictionary {">`initWithDictionary`</SwmToken>: is an instance initializer that creates a <SwmToken path="VENCore/Models/Users/VENUser.m" pos="38:1:1" line-data="    VENUser *newUser        = [[[self class] alloc] init];">`VENUser`</SwmToken> object from a dictionary. It first validates the dictionary, cleanses it, and then assigns values from the dictionary to the <SwmToken path="VENCore/Models/Users/VENUser.m" pos="38:1:1" line-data="    VENUser *newUser        = [[[self class] alloc] init];">`VENUser`</SwmToken>'s properties such as username, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="20:3:3" line-data="        self.firstName      = cleanDictionary[VENUserKeyFirstName];">`firstName`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="21:3:3" line-data="        self.lastName       = cleanDictionary[VENUserKeyLastName];">`lastName`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="22:3:3" line-data="        self.displayName    = cleanDictionary[VENUserKeyDisplayName];">`displayName`</SwmToken>, about, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="24:3:3" line-data="        self.primaryPhone   = cleanDictionary[VENUserKeyPhone];">`primaryPhone`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="25:3:3" line-data="        self.internalId     = cleanDictionary[VENUserKeyInternalId];">`internalId`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="26:3:3" line-data="        self.externalId     = cleanDictionary[VENUserKeyExternalId];">`externalId`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="27:3:3" line-data="        self.dateJoined     = cleanDictionary[VENUserKeyDateJoined];">`dateJoined`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="28:3:3" line-data="        self.primaryEmail   = cleanDictionary[VENUserKeyEmail];">`primaryEmail`</SwmToken>, and <SwmToken path="VENCore/Models/Users/VENUser.m" pos="29:3:3" line-data="        self.profileImageUrl= cleanDictionary[VENUserKeyProfileImageUrl];">`profileImageUrl`</SwmToken>.

```limbo
- (instancetype)initWithDictionary:(NSDictionary *)dictionary {
    self = [super init];

    if (self) {
        
        if (!dictionary || ![dictionary isKindOfClass:[NSDictionary class]]) {
            return self;
        }
        
        NSDictionary *cleanDictionary = [dictionary dictionaryByCleansingResponseDictionary];

        self.username       = cleanDictionary[VENUserKeyUsername];
        self.firstName      = cleanDictionary[VENUserKeyFirstName];
        self.lastName       = cleanDictionary[VENUserKeyLastName];
        self.displayName    = cleanDictionary[VENUserKeyDisplayName];
        self.about          = cleanDictionary[VENUserKeyAbout];
        self.primaryPhone   = cleanDictionary[VENUserKeyPhone];
        self.internalId     = cleanDictionary[VENUserKeyInternalId];
        self.externalId     = cleanDictionary[VENUserKeyExternalId];
        self.dateJoined     = cleanDictionary[VENUserKeyDateJoined];
        self.primaryEmail   = cleanDictionary[VENUserKeyEmail];
        self.profileImageUrl= cleanDictionary[VENUserKeyProfileImageUrl];
    }

    return self;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Models/Users/VENUser.m" line="36">

---

The function <SwmToken path="VENCore/Models/Users/VENUser.m" pos="36:5:5" line-data="- (instancetype)copyWithZone:(NSZone *)zone {">`copyWithZone`</SwmToken>: creates and returns a copy of the <SwmToken path="VENCore/Models/Users/VENUser.m" pos="38:1:1" line-data="    VENUser *newUser        = [[[self class] alloc] init];">`VENUser`</SwmToken> instance. It allocates a new <SwmToken path="VENCore/Models/Users/VENUser.m" pos="38:1:1" line-data="    VENUser *newUser        = [[[self class] alloc] init];">`VENUser`</SwmToken> and copies all the properties from the original instance to the new one, including username, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="41:3:3" line-data="    newUser.firstName       = self.firstName;">`firstName`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="42:3:3" line-data="    newUser.lastName        = self.lastName;">`lastName`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="43:3:3" line-data="    newUser.displayName     = self.displayName;">`displayName`</SwmToken>, about, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="45:3:3" line-data="    newUser.primaryPhone    = self.primaryPhone;">`primaryPhone`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="46:3:3" line-data="    newUser.primaryEmail    = self.primaryEmail;">`primaryEmail`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="47:3:3" line-data="    newUser.internalId      = self.internalId;">`internalId`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="48:3:3" line-data="    newUser.externalId      = self.externalId;">`externalId`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="49:3:3" line-data="    newUser.dateJoined      = self.dateJoined;">`dateJoined`</SwmToken>, and <SwmToken path="VENCore/Models/Users/VENUser.m" pos="50:3:3" line-data="    newUser.profileImageUrl = self.profileImageUrl;">`profileImageUrl`</SwmToken>.

```limbo
- (instancetype)copyWithZone:(NSZone *)zone {

    VENUser *newUser        = [[[self class] alloc] init];

    newUser.username        = self.username;
    newUser.firstName       = self.firstName;
    newUser.lastName        = self.lastName;
    newUser.displayName     = self.displayName;
    newUser.about           = self.about;
    newUser.primaryPhone    = self.primaryPhone;
    newUser.primaryEmail    = self.primaryEmail;
    newUser.internalId      = self.internalId;
    newUser.externalId      = self.externalId;
    newUser.dateJoined      = self.dateJoined;
    newUser.profileImageUrl = self.profileImageUrl;

    return newUser;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Models/Users/VENUser.m" line="56">

---

The function <SwmToken path="VENCore/Models/Users/VENUser.m" pos="56:5:5" line-data="- (BOOL)isEqual:(id)object {">`isEqual`</SwmToken>: compares the current <SwmToken path="VENCore/Models/Users/VENUser.m" pos="61:1:1" line-data="    VENUser *comparisonUser = (VENUser *)object;">`VENUser`</SwmToken> instance with another object to determine equality. It returns YES if the other object is also a <SwmToken path="VENCore/Models/Users/VENUser.m" pos="61:1:1" line-data="    VENUser *comparisonUser = (VENUser *)object;">`VENUser`</SwmToken> and their <SwmToken path="VENCore/Models/Users/VENUser.m" pos="63:10:10" line-data="    BOOL result = [self.externalId isEqualToString:comparisonUser.externalId];">`externalId`</SwmToken> properties are equal, otherwise NO.

```limbo
- (BOOL)isEqual:(id)object {

    if ([object class] != [self class]) {
        return NO;
    }
    VENUser *comparisonUser = (VENUser *)object;

    BOOL result = [self.externalId isEqualToString:comparisonUser.externalId];
    return result;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Models/Users/VENUser.m" line="68">

---

The function description returns a string representation of the <SwmToken path="VENCore/Models/Users/VENUser.m" pos="38:1:1" line-data="    VENUser *newUser        = [[[self class] alloc] init];">`VENUser`</SwmToken> instance. It formats the string to include the class name and the dictionary representation of the user's properties.

```limbo
- (NSString *)description {
    return [NSString stringWithFormat:@"%@ :: %@", [self class], [self dictionaryRepresentation]];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Models/Users/VENUser.m" line="88">

---

The function <SwmToken path="VENCore/Models/Users/VENUser.m" pos="88:7:7" line-data="- (NSDictionary *)dictionaryRepresentation {">`dictionaryRepresentation`</SwmToken> returns an <SwmToken path="VENCore/Models/Users/VENUser.m" pos="88:3:3" line-data="- (NSDictionary *)dictionaryRepresentation {">`NSDictionary`</SwmToken> containing the <SwmToken path="VENCore/Models/Users/VENUser.m" pos="38:1:1" line-data="    VENUser *newUser        = [[[self class] alloc] init];">`VENUser`</SwmToken>'s properties as key-value pairs. It includes keys such as username, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="95:6:6" line-data="    if (self.firstName) {">`firstName`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="99:6:6" line-data="    if (self.lastName) {">`lastName`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="103:6:6" line-data="    if (self.displayName) {">`displayName`</SwmToken>, about, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="111:6:6" line-data="    if (self.primaryPhone) {">`primaryPhone`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="115:6:6" line-data="    if (self.primaryEmail) {">`primaryEmail`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="119:6:6" line-data="    if (self.internalId) {">`internalId`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="123:6:6" line-data="    if (self.externalId) {">`externalId`</SwmToken>, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="127:6:6" line-data="    if (self.profileImageUrl) {">`profileImageUrl`</SwmToken>, and <SwmToken path="VENCore/Models/Users/VENUser.m" pos="131:6:6" line-data="    if (self.dateJoined) {">`dateJoined`</SwmToken>, but only includes those properties that are non-nil.

```limbo
- (NSDictionary *)dictionaryRepresentation {
    NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

    if (self.username) {
        dictionary[VENUserKeyUsername] = self.username;
    }

    if (self.firstName) {
        dictionary[VENUserKeyFirstName] = self.firstName;
    }

    if (self.lastName) {
        dictionary[VENUserKeyLastName] = self.lastName;
    }

    if (self.displayName) {
        dictionary[VENUserKeyDisplayName] = self.displayName;
    }

    if (self.about) {
        dictionary[VENUserKeyAbout] = self.about;
    }

    if (self.primaryPhone) {
        dictionary[VENUserKeyPhone] = self.primaryPhone;
    }

    if (self.primaryEmail) {
        dictionary[VENUserKeyEmail] = self.primaryEmail;
    }

    if (self.internalId) {
        dictionary[VENUserKeyInternalId] = self.internalId;
    }

    if (self.externalId) {
        dictionary[VENUserKeyExternalId] = self.externalId;
    }

    if (self.profileImageUrl) {
        dictionary[VENUserKeyProfileImageUrl] = self.profileImageUrl;
    }

    if (self.dateJoined) {
        dictionary[VENUserKeyDateJoined] = self.dateJoined;
    }

    return dictionary;
}
```

---

</SwmSnippet>

# Usage

## <SwmToken path="VENCore/Models/Users/VENUser.m" pos="73:5:5" line-data="+ (BOOL)canInitWithDictionary:(NSDictionary *)dictionary {">`canInitWithDictionary`</SwmToken>:

The method <SwmToken path="VENCore/Models/Users/VENUser.m" pos="73:5:5" line-data="+ (BOOL)canInitWithDictionary:(NSDictionary *)dictionary {">`canInitWithDictionary`</SwmToken>: is used to check whether an object can be properly initialized using the data provided in a dictionary. It returns a boolean indicating if the dictionary contains sufficient or valid information for initialization.

## Usage in VENUserSpec

In the VENUserSpec unit test file, <SwmToken path="VENCore/Models/Users/VENUser.m" pos="73:5:5" line-data="+ (BOOL)canInitWithDictionary:(NSDictionary *)dictionary {">`canInitWithDictionary`</SwmToken>: is tested to ensure it returns NO when given a nil or empty dictionary. This confirms that the method correctly identifies invalid or insufficient input data and prevents initialization in such cases.

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBdmVubW9fVkVOQ29yZSUzQSUzQW11ZGFzaW4x" repo-name="venmo_VENCore"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
