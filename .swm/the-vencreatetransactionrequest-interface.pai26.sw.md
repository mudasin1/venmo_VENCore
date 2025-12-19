---
title: The VENCreateTransactionRequest interface
---
This document covers the interface <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="1:4:4" line-data="#import &quot;VENCreateTransactionRequest.h&quot;">`VENCreateTransactionRequest`</SwmToken>. We will explain:

1. What <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="1:4:4" line-data="#import &quot;VENCreateTransactionRequest.h&quot;">`VENCreateTransactionRequest`</SwmToken> is and its purpose.
2. The variables and functions defined in <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="1:4:4" line-data="#import &quot;VENCreateTransactionRequest.h&quot;">`VENCreateTransactionRequest`</SwmToken>, including their roles and implementations.

# What is <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="1:4:4" line-data="#import &quot;VENCreateTransactionRequest.h&quot;">`VENCreateTransactionRequest`</SwmToken>

<SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="1:4:4" line-data="#import &quot;VENCreateTransactionRequest.h&quot;">`VENCreateTransactionRequest`</SwmToken> is a class in <SwmPath>[VENCore/…/Transactions/VENCreateTransactionRequest.m](VENCore/Models/Transactions/VENCreateTransactionRequest.m)</SwmPath> that represents a request to create a transaction. It manages the data and logic needed to prepare and send transaction creation requests to the backend API. This class handles multiple transaction targets, validates the request readiness, and manages the sending process including success and failure callbacks.

<SwmSnippet path="/VENCore/Models/Transactions/VENCreateTransactionRequest.m" line="18">

---

The <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="18:5:5" line-data="- (id)init {">`init`</SwmToken> function initializes a new instance of <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="1:4:4" line-data="#import &quot;VENCreateTransactionRequest.h&quot;">`VENCreateTransactionRequest`</SwmToken>. It sets up the <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="21:3:3" line-data="        self.mutableTargets = [[NSMutableOrderedSet alloc] init];">`mutableTargets`</SwmToken> property as an empty <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="21:9:9" line-data="        self.mutableTargets = [[NSMutableOrderedSet alloc] init];">`NSMutableOrderedSet`</SwmToken> to hold transaction targets.

```limbo
- (id)init {
    self = [super init];
    if (self) {
        self.mutableTargets = [[NSMutableOrderedSet alloc] init];
    }
    return self;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Models/Transactions/VENCreateTransactionRequest.m" line="26">

---

The <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="26:5:5" line-data="- (BOOL)readyToSend {">`readyToSend`</SwmToken> function checks if the request is ready to be sent. It returns NO if there are no targets, the note is empty, or the transaction type is unknown; otherwise, it returns YES.

```limbo
- (BOOL)readyToSend {
    if (![self.mutableTargets count] ||
        ![self.note hasContent] ||
        self.transactionType == VENTransactionTypeUnknown) {
        return NO;
    }
    return YES;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Models/Transactions/VENCreateTransactionRequest.m" line="35">

---

The <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="35:5:5" line-data="- (void)sendWithSuccess:(void(^)(NSArray *sentTransactions,">`sendWithSuccess`</SwmToken>`:`<SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="37:1:1" line-data="                failure:(void(^)(NSArray *sentTransactions,">`failure`</SwmToken>`:` function initiates sending the transaction request. It calls <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="40:4:4" line-data="    [self sendTargets:[self.targets mutableCopy]">`sendTargets`</SwmToken>`:`<SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="35:16:16" line-data="- (void)sendWithSuccess:(void(^)(NSArray *sentTransactions,">`sentTransactions`</SwmToken>`:`<SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="42:1:1" line-data="          withSuccess:successBlock">`withSuccess`</SwmToken>`:`<SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="37:1:1" line-data="                failure:(void(^)(NSArray *sentTransactions,">`failure`</SwmToken>`:` with a mutable copy of the current targets and passes along the success and failure callback blocks.

```limbo
- (void)sendWithSuccess:(void(^)(NSArray *sentTransactions,
                                 VENHTTPResponse *response))successBlock
                failure:(void(^)(NSArray *sentTransactions,
                                 VENHTTPResponse *response,
                                 NSError *error))failureBlock {
    [self sendTargets:[self.targets mutableCopy]
     sentTransactions:nil
          withSuccess:successBlock
              failure:failureBlock];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Models/Transactions/VENCreateTransactionRequest.m" line="47">

---

The <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="47:5:5" line-data="- (void)sendTargets:(NSMutableOrderedSet *)targets">`sendTargets`</SwmToken>`:`<SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="48:1:1" line-data="   sentTransactions:(NSMutableArray *)sentTransactions">`sentTransactions`</SwmToken>`:`<SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="49:1:1" line-data="        withSuccess:(void (^)(NSArray *sentTransactions,">`withSuccess`</SwmToken>`:`<SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="51:1:1" line-data="            failure:(void(^)(NSArray *sentTransactions,">`failure`</SwmToken>`:` function handles sending transactions to each target sequentially. It recursively processes the targets array, sending a POST request for each target with appropriate parameters. On success, it adds the new transaction to <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="48:1:1" line-data="   sentTransactions:(NSMutableArray *)sentTransactions">`sentTransactions`</SwmToken> and continues with remaining targets. On failure, it calls the failure block with the current state.

```limbo
- (void)sendTargets:(NSMutableOrderedSet *)targets
   sentTransactions:(NSMutableArray *)sentTransactions
        withSuccess:(void (^)(NSArray *sentTransactions,
                              VENHTTPResponse *response))successBlock
            failure:(void(^)(NSArray *sentTransactions,
                             VENHTTPResponse *response,
                             NSError *error))failureBlock {
    if (!sentTransactions) {
        sentTransactions = [[NSMutableArray alloc] init];
    }

    if ([targets count] == 0) {
        if (successBlock) {
            successBlock(sentTransactions, nil);
        }
        return;
    }

    VENTransactionTarget *target = [targets firstObject];
    [targets removeObjectAtIndex:0];
    NSString *accessToken = [VENCore defaultCore].accessToken;
    if (!accessToken) {
        failureBlock(nil, nil, [NSError noAccessTokenError]);
        return;
    }
    NSMutableDictionary *postParameters = [NSMutableDictionary dictionaryWithDictionary:@{@"access_token" : accessToken}];
    [postParameters addEntriesFromDictionary:[self dictionaryWithParametersForTarget:target]];
    [[VENCore defaultCore].httpClient POST:VENAPIPathPayments
                                parameters:postParameters
                                   success:^(VENHTTPResponse *response) {
                                       NSDictionary *data = [response.object objectOrNilForKey:@"data"];
                                       NSDictionary *payment = [data objectOrNilForKey:@"payment"];
                                       VENTransaction *newTransaction;
                                       if (payment) {
                                           newTransaction = [[VENTransaction alloc] initWithDictionary:payment];
                                       }
                                       [sentTransactions addObject:newTransaction];

                                       [self sendTargets:targets
                                        sentTransactions:sentTransactions
                                             withSuccess:successBlock
                                                 failure:failureBlock];
                                   }
                                   failure:^(VENHTTPResponse *response, NSError *error) {
                                       if (failureBlock) {
                                           failureBlock(sentTransactions, response, error);
                                       }
                                   }];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Models/Transactions/VENCreateTransactionRequest.m" line="98">

---

The <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="98:5:6" line-data="- (BOOL)addTransactionTarget:(VENTransactionTarget *)target {">`addTransactionTarget:`</SwmToken> function attempts to add a <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="98:8:8" line-data="- (BOOL)addTransactionTarget:(VENTransactionTarget *)target {">`VENTransactionTarget`</SwmToken> to the <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="106:4:4" line-data="    [self.mutableTargets addObject:target];">`mutableTargets`</SwmToken> set. It validates the target's class, validity, and checks for duplicates before adding. Returns YES if added successfully, NO otherwise.

```limbo
- (BOOL)addTransactionTarget:(VENTransactionTarget *)target {

    if (![target isKindOfClass:[VENTransactionTarget class]]
        || ![target isValid]
        || [self containsDuplicateOfTarget:target]) {
        return NO;
    }

    [self.mutableTargets addObject:target];
    return YES;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Models/Transactions/VENCreateTransactionRequest.m" line="113">

---

The <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="113:7:7" line-data="- (NSOrderedSet *)targets {">`targets`</SwmToken> function returns an immutable copy of the current transaction targets stored in <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="114:6:6" line-data="   return [self.mutableTargets copy];">`mutableTargets`</SwmToken>.

```limbo
- (NSOrderedSet *)targets {
   return [self.mutableTargets copy];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Models/Transactions/VENCreateTransactionRequest.m" line="117">

---

The <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="117:7:8" line-data="- (NSDictionary *)dictionaryWithParametersForTarget:(VENTransactionTarget *)target {">`dictionaryWithParametersForTarget:`</SwmToken> function constructs a dictionary of parameters for a given transaction target. It sets the recipient type key based on the target type (email, phone, or user ID), formats the amount (negating if it's a charge), includes the note, and sets the audience if specified.

```limbo
- (NSDictionary *)dictionaryWithParametersForTarget:(VENTransactionTarget *)target {
    NSString *recipientTypeKey;
    NSString *audienceString;
    NSString*amountString;
    switch (target.targetType) {
        case VENTargetTypeEmail:
            recipientTypeKey = @"email";
            break;
        case VENTargetTypePhone:
            recipientTypeKey = @"phone";
            break;
        case VENTargetTypeUserId:
            recipientTypeKey = @"user_id";
            break;
        default:
            return nil;
            break;
    }

    switch (self.audience) {
        case VENTransactionAudienceFriends:
            audienceString = @"friends";
            break;
        case VENTransactionAudiencePublic:
            audienceString = @"public";
            break;
        default:
            audienceString = @"private";
            break;
    }
    CGFloat dollarAmount = (CGFloat)target.amount/100.;
    amountString = [NSString stringWithFormat:@"%.2f", dollarAmount];
    if (self.transactionType == VENTransactionTypeCharge) {
        amountString = [@"-" stringByAppendingString:amountString];
    }
    NSMutableDictionary *parameters = [NSMutableDictionary dictionaryWithDictionary:
                                       @{recipientTypeKey: target.handle,
                                         @"note": self.note,
                                         @"amount": amountString}];

    if (self.audience != VENTransactionAudienceUserDefault) {
        [parameters addEntriesFromDictionary:@{@"audience": audienceString}];
    }

    return parameters;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Models/Transactions/VENCreateTransactionRequest.m" line="165">

---

The <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="165:5:6" line-data="- (BOOL)containsDuplicateOfTarget:(VENTransactionTarget *)target {">`containsDuplicateOfTarget:`</SwmToken> function checks if a given target's handle already exists in the current targets. It returns YES if a duplicate is found, NO otherwise.

```limbo
- (BOOL)containsDuplicateOfTarget:(VENTransactionTarget *)target {
    NSString *handle = target.handle;
    for (VENTransactionTarget *currentTarget in self.targets) {
        if ([handle isEqualToString:currentTarget.handle]) {
            return YES;
        }
    }
    return NO;
}
```

---

</SwmSnippet>

# Usage

## <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="1:4:4" line-data="#import &quot;VENCreateTransactionRequest.h&quot;">`VENCreateTransactionRequest`</SwmToken>

<SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="1:4:4" line-data="#import &quot;VENCreateTransactionRequest.h&quot;">`VENCreateTransactionRequest`</SwmToken> is instantiated in test setups to create transaction requests for payments. It is used to simulate both successful and failed payment scenarios by setting parameters such as amount and note before executing the transaction.

## Usage in <SwmPath>[VENCoreIntegrationTests/PaymentSandboxSpec.m](VENCoreIntegrationTests/PaymentSandboxSpec.m)</SwmPath>

In <SwmPath>[VENCoreIntegrationTests/PaymentSandboxSpec.m](VENCoreIntegrationTests/PaymentSandboxSpec.m)</SwmPath>, <SwmToken path="VENCore/Models/Transactions/VENCreateTransactionRequest.m" pos="1:4:4" line-data="#import &quot;VENCreateTransactionRequest.h&quot;">`VENCreateTransactionRequest`</SwmToken> is initialized before each test case to prepare a transaction service instance. It is then used to test making payments to a user ID or an email, verifying both success and failure cases.

## Usage in <SwmPath>[VENCoreUnitTests/…/Transactions/VENCreateTransactionRequestSpec.m](VENCoreUnitTests/Models/Transactions/VENCreateTransactionRequestSpec.m)</SwmPath>

In <SwmPath>[VENCoreUnitTests/…/Transactions/VENCreateTransactionRequestSpec.m](VENCoreUnitTests/Models/Transactions/VENCreateTransactionRequestSpec.m)</SwmPath>, the class is tested with stubbed responses to validate its behavior when sending payments. The tests include checking the parameters generated for each transaction target and ensuring the correct handling of access tokens and error responses.

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBdmVubW9fVkVOQ29yZSUzQSUzQW11ZGFzaW4x" repo-name="venmo_VENCore"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
