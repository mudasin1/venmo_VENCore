---
title: The VENHTTPResponse interface
---
This document covers the interface <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="27:10:10" line-data="    return [NSString stringWithFormat:@&quot;&lt;VENHTTPResponse statusCode:%d body:%@&gt;&quot;, (int)self.statusCode, self.object];">`VENHTTPResponse`</SwmToken>. We will explain:

1. What <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="27:10:10" line-data="    return [NSString stringWithFormat:@&quot;&lt;VENHTTPResponse statusCode:%d body:%@&gt;&quot;, (int)self.statusCode, self.object];">`VENHTTPResponse`</SwmToken> is and its purpose.
2. The variables and functions defined in <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="27:10:10" line-data="    return [NSString stringWithFormat:@&quot;&lt;VENHTTPResponse statusCode:%d body:%@&gt;&quot;, (int)self.statusCode, self.object];">`VENHTTPResponse`</SwmToken>, including their roles and implementations.

# What is <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="27:10:10" line-data="    return [NSString stringWithFormat:@&quot;&lt;VENHTTPResponse statusCode:%d body:%@&gt;&quot;, (int)self.statusCode, self.object];">`VENHTTPResponse`</SwmToken>

<SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="27:10:10" line-data="    return [NSString stringWithFormat:@&quot;&lt;VENHTTPResponse statusCode:%d body:%@&gt;&quot;, (int)self.statusCode, self.object];">`VENHTTPResponse`</SwmToken> is a class implemented in <SwmPath>[VENCore/Networking/VENHTTPResponse.m](VENCore/Networking/VENHTTPResponse.m)</SwmPath> that encapsulates the response from an HTTP request. It is designed to hold the HTTP status code and the response object returned from the server. This class provides a structured way to handle HTTP responses, including checking for errors and extracting error information if present.

<SwmSnippet path="/VENCore/Networking/VENHTTPResponse.m" line="16">

---

The function <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="16:5:5" line-data="- (instancetype)initWithStatusCode:(NSInteger)statusCode responseObject:(id)object {">`initWithStatusCode`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="16:12:12" line-data="- (instancetype)initWithStatusCode:(NSInteger)statusCode responseObject:(id)object {">`responseObject`</SwmToken>`:` is the designated initializer for <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="27:10:10" line-data="    return [NSString stringWithFormat:@&quot;&lt;VENHTTPResponse statusCode:%d body:%@&gt;&quot;, (int)self.statusCode, self.object];">`VENHTTPResponse`</SwmToken>. It initializes an instance with a given HTTP status code and the associated response object. This allows the creation of a <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="27:10:10" line-data="    return [NSString stringWithFormat:@&quot;&lt;VENHTTPResponse statusCode:%d body:%@&gt;&quot;, (int)self.statusCode, self.object];">`VENHTTPResponse`</SwmToken> object that represents the result of an HTTP request.

```limbo
- (instancetype)initWithStatusCode:(NSInteger)statusCode responseObject:(id)object {
    self = [self init];
    if (self) {
        self.statusCode = statusCode;
        self.object = object;
    }
    return self;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTPResponse.m" line="26">

---

The function <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="26:7:7" line-data="- (NSString *)description {">`description`</SwmToken> returns a string representation of the <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="27:10:10" line-data="    return [NSString stringWithFormat:@&quot;&lt;VENHTTPResponse statusCode:%d body:%@&gt;&quot;, (int)self.statusCode, self.object];">`VENHTTPResponse`</SwmToken> instance. It formats the string to include the status code and the body of the response object, which is useful for debugging and logging purposes.

```limbo
- (NSString *)description {
    return [NSString stringWithFormat:@"<VENHTTPResponse statusCode:%d body:%@>", (int)self.statusCode, self.object];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTPResponse.m" line="31">

---

The function <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="31:5:5" line-data="- (BOOL)didError {">`didError`</SwmToken> is a boolean method that indicates whether the HTTP response represents an error. It returns true if the status code is greater than 299, which corresponds to HTTP error status codes.

```limbo
- (BOOL)didError {
    return self.statusCode > 299;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTPResponse.m" line="36">

---

The function <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="36:7:7" line-data="- (NSError *)error {">`error`</SwmToken> returns an <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="36:3:3" line-data="- (NSError *)error {">`NSError`</SwmToken> object if the response indicates an error. It extracts error details from the response object, such as an error message and code, and constructs an <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="36:3:3" line-data="- (NSError *)error {">`NSError`</SwmToken> with a specific error domain. If there is no error, it returns nil. This function helps in translating HTTP response errors into <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="36:3:3" line-data="- (NSError *)error {">`NSError`</SwmToken> objects for easier error handling.

```limbo
- (NSError *)error {
    if (![self didError]) {
        return nil;
    }

    NSDictionary *errorObject = [self.object objectOrNilForKey:@"error"];
    NSString *message = [errorObject stringForKey:@"message"];
    NSError *error;
    if (message) {
        NSString *codeString = [errorObject objectOrNilForKey:@"code"] ?: [errorObject[@"errors"] lastObject][@"error_code"];
        NSInteger code = [codeString integerValue];
        error = [NSError errorWithDomain:VENErrorDomainHTTPResponse
                                    code:code
                             description:message
                      recoverySuggestion:nil];
    }

    return error;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTPResponse.m" line="9">

---

The variable <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="9:15:15" line-data="@property (nonatomic, readwrite, strong) id object;">`object`</SwmToken> is a strong, readwrite property that holds the response object returned from the HTTP request. It can be any type of object representing the response payload.

```limbo
@property (nonatomic, readwrite, strong) id object;
@property (nonatomic, readwrite, assign) NSInteger statusCode;
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTPResponse.m" line="10">

---

The variable <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="10:15:15" line-data="@property (nonatomic, readwrite, assign) NSInteger statusCode;">`statusCode`</SwmToken> is an <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="10:13:13" line-data="@property (nonatomic, readwrite, assign) NSInteger statusCode;">`NSInteger`</SwmToken> property that stores the HTTP status code of the response. It is readwrite and used to determine the success or failure of the HTTP request.

```limbo
@property (nonatomic, readwrite, assign) NSInteger statusCode;

```

---

</SwmSnippet>

# Usage

## <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="27:10:10" line-data="    return [NSString stringWithFormat:@&quot;&lt;VENHTTPResponse statusCode:%d body:%@&gt;&quot;, (int)self.statusCode, self.object];">`VENHTTPResponse`</SwmToken>

<SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="27:10:10" line-data="    return [NSString stringWithFormat:@&quot;&lt;VENHTTPResponse statusCode:%d body:%@&gt;&quot;, (int)self.statusCode, self.object];">`VENHTTPResponse`</SwmToken> is commonly used as the response object in network request callbacks, allowing access to the returned data or error information. For example, in <SwmPath>[VENCore/…/Users/VENUser.m](VENCore/Models/Users/VENUser.m)</SwmPath>, it is used in success blocks to extract user data from the response object and in failure blocks to handle errors.

## Usage in VENUser

In <SwmPath>[VENCore/…/Users/VENUser.m](VENCore/Models/Users/VENUser.m)</SwmPath>, <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="27:10:10" line-data="    return [NSString stringWithFormat:@&quot;&lt;VENHTTPResponse statusCode:%d body:%@&gt;&quot;, (int)self.statusCode, self.object];">`VENHTTPResponse`</SwmToken> is passed to success and failure blocks of HTTP GET requests to fetch user details and friends lists. The response object provides the payload data which is then converted into dictionaries or arrays for further processing.

## Usage in VENCreateTransactionRequest

<SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="27:10:10" line-data="    return [NSString stringWithFormat:@&quot;&lt;VENHTTPResponse statusCode:%d body:%@&gt;&quot;, (int)self.statusCode, self.object];">`VENHTTPResponse`</SwmToken> is used in <SwmPath>[VENCore/…/Transactions/VENCreateTransactionRequest.m](VENCore/Models/Transactions/VENCreateTransactionRequest.m)</SwmPath> to handle the results of POST requests when sending transaction data. The success block receives the response to parse payment information, while the failure block handles any errors returned.

## Usage in VENHTTP

<SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="27:10:10" line-data="    return [NSString stringWithFormat:@&quot;&lt;VENHTTPResponse statusCode:%d body:%@&gt;&quot;, (int)self.statusCode, self.object];">`VENHTTPResponse`</SwmToken> is the type of the response parameter in the success and failure blocks of HTTP methods like GET and POST in <SwmPath>[VENCore/Networking/VENHTTP.m](VENCore/Networking/VENHTTP.m)</SwmPath>, encapsulating the HTTP response details for the caller.

## Usage in Integration Tests

In <SwmPath>[VENCoreIntegrationTests/PaymentSandboxSpec.m](VENCoreIntegrationTests/PaymentSandboxSpec.m)</SwmPath>, <SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="27:10:10" line-data="    return [NSString stringWithFormat:@&quot;&lt;VENHTTPResponse statusCode:%d body:%@&gt;&quot;, (int)self.statusCode, self.object];">`VENHTTPResponse`</SwmToken> is used in success and failure callbacks of transaction service methods to verify transaction statuses and handle errors during testing.

## Usage in Unit Tests

<SwmToken path="VENCore/Networking/VENHTTPResponse.m" pos="27:10:10" line-data="    return [NSString stringWithFormat:@&quot;&lt;VENHTTPResponse statusCode:%d body:%@&gt;&quot;, (int)self.statusCode, self.object];">`VENHTTPResponse`</SwmToken> is used in unit tests such as <SwmPath>[VENCoreUnitTests/Networking/VENHTTPSpec.m](VENCoreUnitTests/Networking/VENHTTPSpec.m)</SwmPath> and <SwmPath>[VENCoreUnitTests/…/Transactions/VENCreateTransactionRequestSpec.m](VENCoreUnitTests/Models/Transactions/VENCreateTransactionRequestSpec.m)</SwmPath> to simulate HTTP responses and verify request parsing and transaction creation logic.

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBdmVubW9fVkVOQ29yZSUzQSUzQW11ZGFzaW4x" repo-name="venmo_VENCore"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
