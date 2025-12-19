---
title: The defaultCore class
---
This document will cover the class <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken> in <SwmPath>[VENCore/VENCore.m](VENCore/VENCore.m)</SwmPath>. We will cover:

1. What is <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken>
2. Variables and functions in <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken>

# What is <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken>

The class method <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken> in <SwmPath>[VENCore/VENCore.m](VENCore/VENCore.m)</SwmPath> is a singleton accessor for the shared instance of the <SwmToken path="VENCore/VENCore.m" pos="32:8:8" line-data="+ (void)setDefaultCore:(VENCore *)core {">`VENCore`</SwmToken> class. It provides a global point of access to a single <SwmToken path="VENCore/VENCore.m" pos="32:8:8" line-data="+ (void)setDefaultCore:(VENCore *)core {">`VENCore`</SwmToken> object, which is used to manage core functionalities such as network communication with the Venmo API. This method returns the shared instance stored in a static variable, allowing consistent and centralized usage of <SwmToken path="VENCore/VENCore.m" pos="32:8:8" line-data="+ (void)setDefaultCore:(VENCore *)core {">`VENCore`</SwmToken> throughout the application.

<SwmSnippet path="/VENCore/VENCore.m" line="13">

---

The function <SwmToken path="VENCore/VENCore.m" pos="13:5:5" line-data="- (instancetype)init {">`init`</SwmToken> is the default initializer for <SwmToken path="VENCore/VENCore.m" pos="32:8:8" line-data="+ (void)setDefaultCore:(VENCore *)core {">`VENCore`</SwmToken>. It initializes the instance by calling the designated initializer <SwmToken path="VENCore/VENCore.m" pos="14:6:7" line-data="    return [self initWithBaseURL:[NSURL URLWithString:VENAPIBaseURL]];">`initWithBaseURL:`</SwmToken> with a predefined base URL for the Venmo API. This ensures that every <SwmToken path="VENCore/VENCore.m" pos="32:8:8" line-data="+ (void)setDefaultCore:(VENCore *)core {">`VENCore`</SwmToken> instance is configured with the correct API endpoint upon creation.

```limbo
- (instancetype)init {
    return [self initWithBaseURL:[NSURL URLWithString:VENAPIBaseURL]];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/VENCore.m" line="17">

---

The function <SwmToken path="VENCore/VENCore.m" pos="17:5:6" line-data="- (instancetype)initWithBaseURL:(NSURL *)baseURL {">`initWithBaseURL:`</SwmToken> is the designated initializer for <SwmToken path="VENCore/VENCore.m" pos="32:8:8" line-data="+ (void)setDefaultCore:(VENCore *)core {">`VENCore`</SwmToken>. It initializes the instance with a specific base URL by creating an internal HTTP client (<SwmToken path="VENCore/VENCore.m" pos="20:9:9" line-data="        self.httpClient = [[VENHTTP alloc] initWithBaseURL:baseURL];">`VENHTTP`</SwmToken>) configured to communicate with that URL. This allows flexibility in specifying different API endpoints if needed.

```limbo
- (instancetype)initWithBaseURL:(NSURL *)baseURL {
    self = [super init];
    if (self) {
        self.httpClient = [[VENHTTP alloc] initWithBaseURL:baseURL];
    }
    return self;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/VENCore.m" line="26">

---

The function <SwmToken path="VENCore/VENCore.m" pos="26:5:6" line-data="- (void)setAccessToken:(NSString *)accessToken {">`setAccessToken:`</SwmToken> sets the access token used for authentication in API requests. It stores the token in an instance variable and also passes it to the internal HTTP client to ensure that all network requests include the proper authorization credentials.

```limbo
- (void)setAccessToken:(NSString *)accessToken {
    _accessToken = accessToken;
    [self.httpClient setAccessToken:accessToken];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/VENCore.m" line="32">

---

The class function <SwmToken path="VENCore/VENCore.m" pos="32:5:6" line-data="+ (void)setDefaultCore:(VENCore *)core {">`setDefaultCore:`</SwmToken> sets the shared singleton instance of <SwmToken path="VENCore/VENCore.m" pos="32:8:8" line-data="+ (void)setDefaultCore:(VENCore *)core {">`VENCore`</SwmToken>. This allows the application to define or replace the global <SwmToken path="VENCore/VENCore.m" pos="32:8:8" line-data="+ (void)setDefaultCore:(VENCore *)core {">`VENCore`</SwmToken> instance that will be returned by <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken>.

```limbo
+ (void)setDefaultCore:(VENCore *)core {
    sharedInstance = core;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/VENCore.m" line="37">

---

The class function <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken> returns the shared singleton instance of <SwmToken path="VENCore/VENCore.m" pos="32:8:8" line-data="+ (void)setDefaultCore:(VENCore *)core {">`VENCore`</SwmToken> stored in the static variable <SwmToken path="VENCore/VENCore.m" pos="38:3:3" line-data="    return sharedInstance;">`sharedInstance`</SwmToken>. This method provides global access to the core functionality managed by <SwmToken path="VENCore/VENCore.m" pos="32:8:8" line-data="+ (void)setDefaultCore:(VENCore *)core {">`VENCore`</SwmToken>.

```limbo
+ (instancetype)defaultCore {
    return sharedInstance;
}
```

---

</SwmSnippet>

# Usage

## <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken> in Unit Tests

In unit tests, <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken> is used to retrieve the shared <SwmToken path="VENCore/VENCore.m" pos="32:8:8" line-data="+ (void)setDefaultCore:(VENCore *)core {">`VENCore`</SwmToken> instance to access its HTTP client and default headers. This allows tests to stub network requests consistently by using the same core configuration. For example, VENTestUtilities uses <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken> to get the HTTP client's default headers and to construct URLs for stubbing GET and POST requests.

## <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken> in VENCoreSpec Tests

The VENCoreSpec tests verify the behavior of the <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken> singleton itself. They test setting a new <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken> instance, retrieving it, overwriting an existing one, and releasing the old instance. This ensures that the shared core instance management behaves correctly during the app lifecycle.

## <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken> in User Model

Within the VENUser model, <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken> is used to perform network requests related to user data. It accesses the shared HTTP client to send GET requests for fetching user details and friends lists. This usage centralizes network communication through the shared core instance, ensuring consistent API interactions.

## <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken> in Transaction Requests

In VENCreateTransactionRequest, <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken> provides the access token and HTTP client needed to post payment requests. The access token is retrieved from <SwmToken path="VENCore/VENCore.m" pos="37:5:5" line-data="+ (instancetype)defaultCore {">`defaultCore`</SwmToken> to authorize the transaction, and the HTTP client is used to send the POST request to the payments API endpoint.

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBdmVubW9fVkVOQ29yZSUzQSUzQW11ZGFzaW4x" repo-name="venmo_VENCore"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
