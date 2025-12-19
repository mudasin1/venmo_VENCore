---
title: The VENTransaction interface
---
This document will cover the interface <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken>. We will cover:

1. What is <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken>
2. Variables and functions of <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken>

# What is <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken>

<SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken> is a model class in <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="2:4:4" line-data="#import &quot;VENCore.h&quot;">`VENCore`</SwmToken> that represents a financial transaction. It encapsulates the details of a transaction such as the transaction ID, the involved parties, the transaction type, status, audience, and associated notes. This class is used to create transaction objects from dictionary data, typically parsed from API responses, and provides a structured way to access transaction information within the application.

<SwmSnippet path="/VENCore/Models/Transactions/VENTransaction.m" line="28">

---

The class method <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="28:5:6" line-data="+ (BOOL)canInitWithDictionary:(NSDictionary *)dictionary {">`canInitWithDictionary:`</SwmToken> determines if a given dictionary contains all the required keys and valid values to initialize a <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken> instance. It checks for the presence and validity of keys like amount, note, actor, transaction ID, and target, returning YES if the dictionary is suitable for initialization, otherwise NO.

```limbo
+ (BOOL)canInitWithDictionary:(NSDictionary *)dictionary {
    NSArray *requiredKeys = @[VENTransactionAmountKey, VENTransactionNoteKey, VENTransactionActorKey, VENTransactionIDKey, VENTransactionTargetKey];
    for (NSString *key in requiredKeys) {
        if (!dictionary[key] || [dictionary[key] isKindOfClass:[NSNull class]]
            || ([dictionary[key] respondsToSelector:@selector(isEqualToString:)]
                && [dictionary[key] isEqualToString:@""])) {
                return NO;
            }
    }
    return YES;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Models/Transactions/VENTransaction.m" line="42">

---

The instance method <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="42:5:6" line-data="- (instancetype)initWithDictionary:(NSDictionary *)dictionary {">`initWithDictionary:`</SwmToken> initializes a <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken> object using data from a dictionary. It first cleanses the dictionary, then extracts and sets properties such as <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="54:3:3" line-data="        self.transactionID      = cleanDictionary[VENTransactionIDKey];">`transactionID`</SwmToken>, note, <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="57:4:4" line-data="        NSString *transactionType       = cleanDictionary[VENTransactionTypeKey];">`transactionType`</SwmToken>, status, audience, actor (as a <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="104:7:7" line-data="        // Set up VENUser actor">`VENUser`</SwmToken>), and target (as a <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="117:5:5" line-data="        if ([VENTransactionTarget canInitWithDictionary:targetDictionary]) {">`VENTransactionTarget`</SwmToken>). It converts string representations of type, status, and audience into their respective enumerations for internal use.

```limbo
- (instancetype)initWithDictionary:(NSDictionary *)dictionary {

    self = [super init];
    
    if (self) {
        if (!dictionary || ![dictionary isKindOfClass:[NSDictionary class]]) {
            return self;
        }
        
        NSDictionary *cleanDictionary = [dictionary dictionaryByCleansingResponseDictionary];
        
        // Main Transaction Body
        self.transactionID      = cleanDictionary[VENTransactionIDKey];
        self.note               = cleanDictionary[VENTransactionNoteKey];
        
        NSString *transactionType       = cleanDictionary[VENTransactionTypeKey];
        NSString *transactionStatus     = cleanDictionary[VENTransactionStatusKey];
        NSString *transactionAudience   = cleanDictionary[VENTransactionAudienceKey];
        

        // Set transaction type enumeration
        if ([transactionType isEqualToString:VENTransactionTypeStrings[VENTransactionTypeCharge]]) {
            self.transactionType = VENTransactionTypeCharge;
        }
        else if ([transactionType isEqualToString:VENTransactionTypeStrings[VENTransactionTypePay]]) {
            self.transactionType = VENTransactionTypePay;
        }
        else {
            self.transactionType = VENTransactionTypeUnknown;
        }
        
        
        // Set status enumeration
        if ([transactionStatus isEqualToString:VENTransactionStatusStrings[VENTransactionStatusPending]]) {
            self.status = VENTransactionStatusPending;
        }
        else if ([transactionStatus isEqualToString:VENTransactionStatusStrings[VENTransactionStatusSettled]]) {
            self.status = VENTransactionStatusSettled;
        }
        else if ([transactionStatus isEqualToString:VENTransactionStatusStrings[VENTransactionStatusFailed]]) {
            self.status = VENTransactionStatusFailed;
        }
        else {
            self.status = VENTransactionStatusUnknown;
        }
        
        
        // Set audience enumeration
        if ([transactionAudience isEqualToString:VENTransactionAudienceStrings[VENTransactionAudiencePublic]]) {
            self.audience = VENTransactionAudiencePublic;
        }
        else if ([transactionAudience isEqualToString:VENTransactionAudienceStrings[VENTransactionAudienceFriends]]) {
            self.audience = VENTransactionAudienceFriends;
        }
        else if ([transactionAudience isEqualToString:VENTransactionAudienceStrings[VENTransactionAudiencePrivate]]) {
            self.audience = VENTransactionAudiencePrivate;
        }
        else {
            self.audience = VENTransactionAudiencePrivate;
        }
        
        
        // Set up VENUser actor
        NSDictionary *userDictionary = cleanDictionary[VENTransactionActorKey];
        if ([VENUser canInitWithDictionary:userDictionary]) {
            VENUser *user = [[VENUser alloc] initWithDictionary:userDictionary];
            self.actor = user;
        }
        
        
        // Set up VENTransactionTargets
        NSMutableDictionary *targetDictionary = [cleanDictionary[VENTransactionTargetKey] mutableCopy];
        if (cleanDictionary[VENTransactionAmountKey]) {
            targetDictionary[VENTransactionAmountKey] = cleanDictionary[VENTransactionAmountKey];
        }
        if ([VENTransactionTarget canInitWithDictionary:targetDictionary]) {
            VENTransactionTarget *target = [[VENTransactionTarget alloc] initWithDictionary:targetDictionary];
            self.target = target;
        }
    }
    return self;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Models/Transactions/VENTransaction.m" line="128">

---

The instance method <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="128:5:6" line-data="- (BOOL)isEqual:(id)object {">`isEqual:`</SwmToken> compares the current <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken> object with another object to determine equality. It returns YES if the other object is a <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken> with the same <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="131:8:8" line-data="    if (![otherObject.transactionID isEqualToString:self.transactionID]">`transactionID`</SwmToken> and <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="132:5:5" line-data="        || otherObject.transactionType != self.transactionType) {">`transactionType`</SwmToken>; otherwise, it returns NO. This method is useful for comparing transaction instances to avoid duplicates or identify specific transactions.

```limbo
- (BOOL)isEqual:(id)object {
    VENTransaction *otherObject = (VENTransaction *)object;

    if (![otherObject.transactionID isEqualToString:self.transactionID]
        || otherObject.transactionType != self.transactionType) {
        return NO;
    }

    return YES;
}
```

---

</SwmSnippet>

# Usage

## <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken>

<SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken> is used in various test cases to verify transaction processing outcomes such as settled, pending, or failed statuses. For example, in the PaymentSandboxSpec tests, instances of <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken> are retrieved from the transaction service's sendWithSuccess callback to assert the transaction's status after sending.

In unit tests like VENTransactionSpec, <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken> is validated for proper initialization from dictionaries, ensuring that only valid transaction data with required fields like transaction ID can instantiate the class. This helps maintain data integrity when creating transaction objects.

<SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken> instances are also created from server responses, as seen in VENCreateTransactionRequest where a new transaction object is initialized from a dictionary extracted from the HTTP response payload. This demonstrates how <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken> acts as a model for transaction data received from network operations.

Overall, <SwmToken path="VENCore/Models/Transactions/VENTransaction.m" pos="129:1:1" line-data="    VENTransaction *otherObject = (VENTransaction *)object;">`VENTransaction`</SwmToken> serves as a core data model representing payment transactions, supporting status checks, initialization from dictionaries, and integration with network responses, making it central to transaction handling and testing workflows.

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBdmVubW9fVkVOQ29yZSUzQSUzQW11ZGFzaW4x" repo-name="venmo_VENCore"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
