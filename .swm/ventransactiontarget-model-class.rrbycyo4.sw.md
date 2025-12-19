---
title: VENTransactionTarget Model Class
---
# introduction

This document explains the main design decisions behind the <SwmToken path="VENCore/Models/Transactions/VENTransactionTarget.m" pos="10:3:3" line-data="@implementation VENTransactionTarget">`VENTransactionTarget`</SwmToken> model class. It covers:

1. How the class validates input dictionaries to decide if they can initialize a transaction target.
2. How the class initializes itself from a dictionary or from explicit parameters.
3. How the class converts itself back into a dictionary representation.
4. How the class checks its own validity.
5. How equality between two transaction targets is determined.

# validating input dictionaries

<SwmSnippet path="/VENCore/Models/Transactions/VENTransactionTarget.m" line="10">

---

The class method <SwmToken path="VENCore/Models/Transactions/VENTransactionTarget.m" pos="14:5:6" line-data="+ (BOOL)canInitWithDictionary:(NSDictionary *)dictionary {">`canInitWithDictionary:`</SwmToken> checks if a dictionary has the required keys and valid values to create a transaction target. It requires the dictionary to have an amount and a target type key, and verifies the target type is one of the allowed types (phone, email, or user). It also ensures the amount is a valid number and converts it to an integer representation in cents. This method prevents invalid or incomplete data from being used to create a transaction target, enforcing data integrity early.

```limbo
@implementation VENTransactionTarget

#pragma mark - Class Methods

+ (BOOL)canInitWithDictionary:(NSDictionary *)dictionary {

    NSArray *requiredKeys = @[VENTransactionAmountKey, VENTransactionTargetTypeKey];

    for (NSString *key in requiredKeys) {
        if (!dictionary[key] || [dictionary[key] isKindOfClass:[NSNull class]]) {
            return NO;
        }
    }

    NSArray *validTargetTypes = @[VENTransactionTargetPhoneKey, VENTransactionTargetEmailKey, VENTransactionTargetUserKey];

    NSString *targetType = dictionary[VENTransactionTargetTypeKey];
    if (![validTargetTypes containsObject:targetType]) {
        return NO;
    }

    id amount = dictionary[VENTransactionAmountKey];

    if ([amount isKindOfClass:[NSString class]] && ![amount intValue]) {
        return NO;
    }
    else if ([amount respondsToSelector:@selector(doubleValue)]) {
        amount = @([amount doubleValue] * 100.);
    }
    else {
        return NO;
    }

    return YES;
}
```

---

</SwmSnippet>

# initializing the model

There are two initializers:

- <SwmToken path="VENCore/Models/Transactions/VENTransactionTarget.m" pos="49:5:5" line-data="- (instancetype)initWithHandle:(NSString *)phoneEmailOrUserID amount:(NSInteger)amount {">`initWithHandle`</SwmToken>`:`<SwmToken path="VENCore/Models/Transactions/VENTransactionTarget.m" pos="31:3:3" line-data="    id amount = dictionary[VENTransactionAmountKey];">`amount`</SwmToken>`:` takes a string handle (phone, email, or user ID) and an integer amount in cents. It rejects negative amounts and derives the target type from the handle format.
- <SwmToken path="VENCore/Models/Transactions/VENTransactionTarget.m" pos="64:5:6" line-data="- (instancetype)initWithDictionary:(NSDictionary *)dictionary {">`initWithDictionary:`</SwmToken> takes a dictionary, cleans it, and extracts the target type and amount. It maps string target types to internal enum values and converts the amount to cents.

<SwmSnippet path="/VENCore/Models/Transactions/VENTransactionTarget.m" line="47">

---

This dual approach supports flexible creation from raw data or explicit parameters, while normalizing and validating inputs.

```limbo
#pragma mark - Public Instance Methods

- (instancetype)initWithHandle:(NSString *)phoneEmailOrUserID amount:(NSInteger)amount {
    if (amount < 0) {
        return nil;
    }

    self = [super init];
    if (self) {
        self.handle = phoneEmailOrUserID;
        self.amount = amount;
        self.targetType = [phoneEmailOrUserID targetType];
    }
    return self;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Models/Transactions/VENTransactionTarget.m" line="64">

---

&nbsp;

```limbo
- (instancetype)initWithDictionary:(NSDictionary *)dictionary {
    
    self = [super init];
    if (self) {
        
        if (!dictionary || ![dictionary isKindOfClass:[NSDictionary class]]) {
            return self;
        }
        
        NSDictionary *cleanDictionary = [dictionary dictionaryByCleansingResponseDictionary];

        NSString *targetType = cleanDictionary[VENTransactionTargetTypeKey];
        
        if ([targetType isEqualToString:VENTransactionTargetEmailKey]) {
            self.targetType = VENTargetTypeEmail;
        }
        else if ([targetType isEqualToString:VENTransactionTargetUserKey]) {
            self.targetType = VENTargetTypeUserId;
        }
        else if ([targetType isEqualToString:VENTransactionTargetPhoneKey]) {
            self.targetType = VENTargetTypePhone;
        }
        else {
            self.targetType = VENTargetTypeUnknown;
        }
        
        self.handle = cleanDictionary[targetType];
        self.amount = (NSUInteger)([cleanDictionary[VENTransactionAmountKey] doubleValue] * (double)100);
    }
    return self;
}
```

---

</SwmSnippet>

# dictionary representation

<SwmSnippet path="/VENCore/Models/Transactions/VENTransactionTarget.m" line="97">

---

The <SwmToken path="VENCore/Models/Transactions/VENTransactionTarget.m" pos="97:7:7" line-data="- (NSDictionary *)dictionaryRepresentation {">`dictionaryRepresentation`</SwmToken> method converts the model back into a dictionary suitable for serialization or API use. It sets the target type key and the corresponding handle key based on the handle’s type. The amount is converted back from cents to a floating-point number. This method ensures the model can be easily converted to a format expected by other parts of the system.

```limbo
- (NSDictionary *)dictionaryRepresentation {
    NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];
    if (self.handle) {
        //dictionary[VENTransactionTargetTypeKey] =
        VENTargetType targetType = [self.handle targetType];
        switch (targetType) {
            case VENTargetTypeEmail:
                dictionary[VENTransactionTargetTypeKey]  = VENTransactionTargetEmailKey;
                dictionary[VENTransactionTargetEmailKey] = self.handle;
                break;
            case VENTargetTypePhone:
                dictionary[VENTransactionTargetTypeKey]  = VENTransactionTargetPhoneKey;
                dictionary[VENTransactionTargetPhoneKey] = self.handle;
                break;
            case VENTargetTypeUserId:
                dictionary[VENTransactionTargetTypeKey]  = VENTransactionTargetUserKey;
                dictionary[VENTransactionTargetUserKey]  = self.handle;
                break;
            default:
                break;
        }
    }

    if (self.amount) {
        dictionary[VENTransactionAmountKey] = @((CGFloat)self.amount/100.);
    }
    
    return dictionary;
}
```

---

</SwmSnippet>

# validity check

<SwmSnippet path="/VENCore/Models/Transactions/VENTransactionTarget.m" line="128">

---

The <SwmToken path="VENCore/Models/Transactions/VENTransactionTarget.m" pos="128:5:5" line-data="- (BOOL)isValid {">`isValid`</SwmToken> method confirms the model has a valid handle (user ID, US phone, or email), a known target type, and a positive amount. This method is a quick way to verify the model is in a usable state before processing or sending it.

```limbo
- (BOOL)isValid {
    BOOL hasValidHandle = [self.handle isUserId] || [self.handle isUSPhone] || [self.handle isEmail];
    return hasValidHandle && self.targetType != VENTargetTypeUnknown && self.amount > 0;
}


#pragma mark - Other Methods
```

---

</SwmSnippet>

# equality comparison

<SwmSnippet path="/VENCore/Models/Transactions/VENTransactionTarget.m" line="136">

---

The <SwmToken path="VENCore/Models/Transactions/VENTransactionTarget.m" pos="143:5:6" line-data="- (BOOL)isEqual:(id)object {">`isEqual:`</SwmToken> method compares two transaction targets by their handle and amount. It returns false if the handles differ or the amounts differ. This allows the system to detect duplicates or changes in transaction targets reliably.

```limbo
- (void)setUser:(VENUser *)user {
    _user = user;
    self.handle = user.externalId;
    self.targetType = [self.handle targetType];
}


- (BOOL)isEqual:(id)object {
    if (![object isKindOfClass:[self class]]) {
        return NO;
    }
    
    VENTransactionTarget *otherTarget = (VENTransactionTarget *)object;
    
    if ((otherTarget.handle || self.handle) && ![otherTarget.handle isEqualToString:self.handle]) {
        return NO;
    }
    if (otherTarget.amount != self.amount) {
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
