---
title: The VENHTTP interface
---
This document explains the VENHTTP interface in <SwmPath>[VENCore/Networking/VENHTTP.m](VENCore/Networking/VENHTTP.m)</SwmPath>. It covers:

1. What VENHTTP is and its purpose.
2. Detailed explanations of the variables and functions defined in VENHTTP.

# What is VENHTTP

VENHTTP is a networking interface implemented in <SwmPath>[VENCore/Networking/VENHTTP.m](VENCore/Networking/VENHTTP.m)</SwmPath> that provides an abstraction layer for making HTTP requests. It is designed to handle communication with a backend API by managing HTTP sessions, constructing requests with appropriate headers and parameters, and processing responses. VENHTTP supports common HTTP methods such as GET, POST, PUT, and DELETE, and manages authentication tokens and headers to facilitate secure and consistent API interactions.

<SwmSnippet path="/VENCore/Networking/VENHTTP.m" line="24">

---

The function <SwmToken path="VENCore/Networking/VENHTTP.m" pos="24:5:6" line-data="- (instancetype)initWithBaseURL:(NSURL *)baseURL">`initWithBaseURL:`</SwmToken> initializes an instance of VENHTTP with a specified base URL. It sets the base URL property and initializes the <SwmToken path="VENCore/Networking/VENHTTP.m" pos="44:8:8" line-data="        self.session = [NSURLSession sessionWithConfiguration:configuration delegate:self delegateQueue:delegateQueue];">`NSURLSession`</SwmToken> with default headers to prepare for making HTTP requests.

```limbo
- (instancetype)initWithBaseURL:(NSURL *)baseURL
{
    self = [self init];
    if (self) {
        self.baseURL = baseURL;
        [self initializeSessionWithHeaders:self.defaultHeaders];
    }
    return self;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTP.m" line="35">

---

The function <SwmToken path="VENCore/Networking/VENHTTP.m" pos="35:5:6" line-data="- (void)initializeSessionWithHeaders:(NSDictionary *)headers;">`initializeSessionWithHeaders:`</SwmToken> sets up the <SwmToken path="VENCore/Networking/VENHTTP.m" pos="44:8:8" line-data="        self.session = [NSURLSession sessionWithConfiguration:configuration delegate:self delegateQueue:delegateQueue];">`NSURLSession`</SwmToken> used for network requests. It creates a session configuration with ephemeral settings and applies the provided HTTP headers. If a session already exists, it resets it before creating a new one with the updated headers.

```limbo
- (void)initializeSessionWithHeaders:(NSDictionary *)headers;
{
    void(^createSessionBlock)() = ^() {
        NSURLSessionConfiguration *configuration = [NSURLSessionConfiguration ephemeralSessionConfiguration];
        configuration.HTTPAdditionalHeaders = self.defaultHeaders;

        NSOperationQueue *delegateQueue = [[NSOperationQueue alloc] init];
        delegateQueue.maxConcurrentOperationCount = NSOperationQueueDefaultMaxConcurrentOperationCount;

        self.session = [NSURLSession sessionWithConfiguration:configuration delegate:self delegateQueue:delegateQueue];
    };

    if (self.session) {
        [self.session resetWithCompletionHandler:createSessionBlock];
    }
    else {
        createSessionBlock();
    }
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTP.m" line="56">

---

The function <SwmToken path="VENCore/Networking/VENHTTP.m" pos="56:5:6" line-data="- (void)setProtocolClasses:(NSArray *)protocolClasses {">`setProtocolClasses:`</SwmToken> allows customization of the <SwmToken path="VENCore/Networking/VENHTTP.m" pos="59:8:8" line-data="    self.session = [NSURLSession sessionWithConfiguration:configuration delegate:self delegateQueue:self.session.delegateQueue];">`NSURLSession`</SwmToken>'s protocol classes. It updates the session configuration to use the specified protocol classes and recreates the <SwmToken path="VENCore/Networking/VENHTTP.m" pos="59:8:8" line-data="    self.session = [NSURLSession sessionWithConfiguration:configuration delegate:self delegateQueue:self.session.delegateQueue];">`NSURLSession`</SwmToken> with this configuration.

```limbo
- (void)setProtocolClasses:(NSArray *)protocolClasses {
    NSURLSessionConfiguration *configuration = self.session.configuration;
    configuration.protocolClasses = protocolClasses;
    self.session = [NSURLSession sessionWithConfiguration:configuration delegate:self delegateQueue:self.session.delegateQueue];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTP.m" line="63">

---

The function <SwmToken path="VENCore/Networking/VENHTTP.m" pos="63:5:5" line-data="- (void)GET:(NSString *)path parameters:(NSDictionary *)parameters">`GET`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="63:14:14" line-data="- (void)GET:(NSString *)path parameters:(NSDictionary *)parameters">`parameters`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="64:1:1" line-data="    success:(void(^)(VENHTTPResponse *response))successBlock">`success`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="65:1:1" line-data="    failure:(void(^)(VENHTTPResponse *response, NSError *error))failureBlock">`failure`</SwmToken>`:` performs an HTTP GET request to a given path with specified parameters. It uses the underlying <SwmToken path="VENCore/Networking/VENHTTP.m" pos="67:4:4" line-data="    [self sendRequestWithMethod:@&quot;GET&quot; path:path parameters:parameters success:successBlock failure:failureBlock];">`sendRequestWithMethod`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="63:12:12" line-data="- (void)GET:(NSString *)path parameters:(NSDictionary *)parameters">`path`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="63:14:14" line-data="- (void)GET:(NSString *)path parameters:(NSDictionary *)parameters">`parameters`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="64:1:1" line-data="    success:(void(^)(VENHTTPResponse *response))successBlock">`success`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="65:1:1" line-data="    failure:(void(^)(VENHTTPResponse *response, NSError *error))failureBlock">`failure`</SwmToken>`:` method to execute the request and handle the response asynchronously.

```limbo
- (void)GET:(NSString *)path parameters:(NSDictionary *)parameters
    success:(void(^)(VENHTTPResponse *response))successBlock
    failure:(void(^)(VENHTTPResponse *response, NSError *error))failureBlock
{
    [self sendRequestWithMethod:@"GET" path:path parameters:parameters success:successBlock failure:failureBlock];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTP.m" line="71">

---

The function <SwmToken path="VENCore/Networking/VENHTTP.m" pos="71:5:5" line-data="- (void)POST:(NSString *)path parameters:(NSDictionary *)parameters">`POST`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="71:14:14" line-data="- (void)POST:(NSString *)path parameters:(NSDictionary *)parameters">`parameters`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="72:1:1" line-data="     success:(void(^)(VENHTTPResponse *response))successBlock">`success`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="73:1:1" line-data="     failure:(void(^)(VENHTTPResponse *response, NSError *error))failureBlock">`failure`</SwmToken>`:` performs an HTTP POST request to a given path with specified parameters. It delegates the request execution to the common <SwmToken path="VENCore/Networking/VENHTTP.m" pos="76:4:4" line-data="    [self sendRequestWithMethod:@&quot;POST&quot; path:path parameters:parameters success:successBlock failure:failureBlock];">`sendRequestWithMethod`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="71:12:12" line-data="- (void)POST:(NSString *)path parameters:(NSDictionary *)parameters">`path`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="71:14:14" line-data="- (void)POST:(NSString *)path parameters:(NSDictionary *)parameters">`parameters`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="72:1:1" line-data="     success:(void(^)(VENHTTPResponse *response))successBlock">`success`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="73:1:1" line-data="     failure:(void(^)(VENHTTPResponse *response, NSError *error))failureBlock">`failure`</SwmToken>`:` method.

```limbo
- (void)POST:(NSString *)path parameters:(NSDictionary *)parameters
     success:(void(^)(VENHTTPResponse *response))successBlock
     failure:(void(^)(VENHTTPResponse *response, NSError *error))failureBlock
{
    
    [self sendRequestWithMethod:@"POST" path:path parameters:parameters success:successBlock failure:failureBlock];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTP.m" line="80">

---

The function <SwmToken path="VENCore/Networking/VENHTTP.m" pos="80:5:5" line-data="- (void)PUT:(NSString *)path parameters:(NSDictionary *)parameters">`PUT`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="80:14:14" line-data="- (void)PUT:(NSString *)path parameters:(NSDictionary *)parameters">`parameters`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="81:1:1" line-data="    success:(void (^)(VENHTTPResponse *))successBlock">`success`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="82:1:1" line-data="    failure:(void (^)(VENHTTPResponse *, NSError *))failureBlock">`failure`</SwmToken>`:` performs an HTTP PUT request to a given path with specified parameters. It also uses the shared <SwmToken path="VENCore/Networking/VENHTTP.m" pos="84:4:4" line-data="    [self sendRequestWithMethod:@&quot;PUT&quot; path:path parameters:parameters success:successBlock failure:failureBlock];">`sendRequestWithMethod`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="80:12:12" line-data="- (void)PUT:(NSString *)path parameters:(NSDictionary *)parameters">`path`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="80:14:14" line-data="- (void)PUT:(NSString *)path parameters:(NSDictionary *)parameters">`parameters`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="81:1:1" line-data="    success:(void (^)(VENHTTPResponse *))successBlock">`success`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="82:1:1" line-data="    failure:(void (^)(VENHTTPResponse *, NSError *))failureBlock">`failure`</SwmToken>`:` method for request handling.

```limbo
- (void)PUT:(NSString *)path parameters:(NSDictionary *)parameters
    success:(void (^)(VENHTTPResponse *))successBlock
    failure:(void (^)(VENHTTPResponse *, NSError *))failureBlock
{
    [self sendRequestWithMethod:@"PUT" path:path parameters:parameters success:successBlock failure:failureBlock];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTP.m" line="88">

---

The function <SwmToken path="VENCore/Networking/VENHTTP.m" pos="88:5:5" line-data="- (void)DELETE:(NSString *)path parameters:(NSDictionary *)parameters">`DELETE`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="88:14:14" line-data="- (void)DELETE:(NSString *)path parameters:(NSDictionary *)parameters">`parameters`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="89:1:1" line-data="       success:(void(^)(VENHTTPResponse *response))successBlock">`success`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="90:1:1" line-data="       failure:(void(^)(VENHTTPResponse *response, NSError *error))failureBlock">`failure`</SwmToken>`:` performs an HTTP DELETE request to a given path with specified parameters. It calls the common request sending method to execute the request and process the response.

```limbo
- (void)DELETE:(NSString *)path parameters:(NSDictionary *)parameters
       success:(void(^)(VENHTTPResponse *response))successBlock
       failure:(void(^)(VENHTTPResponse *response, NSError *error))failureBlock
{
    [self sendRequestWithMethod:@"DELETE" path:path parameters:parameters success:successBlock failure:failureBlock];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTP.m" line="99">

---

The function <SwmToken path="VENCore/Networking/VENHTTP.m" pos="99:5:5" line-data="- (void)sendRequestWithMethod:(NSString *)method">`sendRequestWithMethod`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="100:1:1" line-data="                         path:(NSString *)aPath">`path`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="101:1:1" line-data="                   parameters:(NSDictionary *)parameters">`parameters`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="102:1:1" line-data="                      success:(void(^)(VENHTTPResponse *response))successBlock">`success`</SwmToken>`:`<SwmToken path="VENCore/Networking/VENHTTP.m" pos="103:1:1" line-data="                      failure:(void(^)(VENHTTPResponse *response, NSError *error))failureBlock">`failure`</SwmToken>`:` is the core method that constructs and sends an HTTP request with the specified method, path, and parameters. It builds the full URL, encodes parameters appropriately for GET/DELETE or POST/PUT methods, sets headers including authentication tokens, and initiates the <SwmToken path="VENCore/Networking/VENHTTP.m" pos="44:8:8" line-data="        self.session = [NSURLSession sessionWithConfiguration:configuration delegate:self delegateQueue:delegateQueue];">`NSURLSession`</SwmToken> data task. Upon completion, it processes the response, parses JSON, and calls the success or failure callback blocks accordingly.

```limbo
- (void)sendRequestWithMethod:(NSString *)method
                         path:(NSString *)aPath
                   parameters:(NSDictionary *)parameters
                      success:(void(^)(VENHTTPResponse *response))successBlock
                      failure:(void(^)(VENHTTPResponse *response, NSError *error))failureBlock
{
    NSURL *fullPathURL = [self.baseURL URLByAppendingPathComponent:aPath];
    NSURLComponents *components = [NSURLComponents componentsWithString:fullPathURL.absoluteString];

    NSMutableURLRequest *request;

    NSString *percentEncodedQuery = [CMDQueryStringSerialization queryStringWithDictionary:parameters];
    if ([method isEqualToString:@"GET"] || [method isEqualToString:@"DELETE"]) {
        components.percentEncodedQuery = percentEncodedQuery;
        request = [NSMutableURLRequest requestWithURL:components.URL];
    } else {
        request = [NSMutableURLRequest requestWithURL:components.URL];
        NSData *body = [percentEncodedQuery dataUsingEncoding:NSUTF8StringEncoding];
        [request setHTTPBody:body];
        NSDictionary *headers = @{@"Content-Type": @"application/x-www-form-urlencoded; charset=utf-8"};
        [request setAllHTTPHeaderFields:headers];
    }
    // Add headers
    NSMutableDictionary *currentHeaders = [NSMutableDictionary dictionaryWithDictionary:request.allHTTPHeaderFields];
    [currentHeaders addEntriesFromDictionary:[self headersWithAccessToken:self.accessToken]];
    [request setAllHTTPHeaderFields:currentHeaders];

    [request setHTTPMethod:method];

    // Perform the actual request
    NSURLSessionTask *task = [self.session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
        [[self class] handleRequestCompletion:data response:response error:error success:successBlock failure:failureBlock];
    }];
    [task resume];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTP.m" line="220">

---

The function <SwmToken path="VENCore/Networking/VENHTTP.m" pos="220:7:8" line-data="- (NSDictionary *)headersWithAccessToken:(NSString *)accessToken">`headersWithAccessToken:`</SwmToken> generates HTTP headers for requests, including the access token if provided. It also sets an HTTP cookie with the access token for webview compatibility and merges these with default headers.

```limbo
- (NSDictionary *)headersWithAccessToken:(NSString *)accessToken
{
    if (!accessToken) {
        return [self defaultHeaders];
    }

    NSDictionary *cookieProperties = @{ NSHTTPCookieDomain : [self.baseURL host],
                                        NSHTTPCookiePath: @"/",
                                        NSHTTPCookieName: @"api_access_token",
                                        NSHTTPCookieValue: accessToken };
    NSHTTPCookie *cookie = [NSHTTPCookie cookieWithProperties:cookieProperties];
    // Add cookie to shared cookie storage for webview requests
    [[NSHTTPCookieStorage sharedHTTPCookieStorage] setCookie:cookie];
    NSMutableDictionary *headers = [NSMutableDictionary dictionaryWithDictionary:[NSHTTPCookie requestHeaderFieldsWithCookies:@[cookie]]];
    [headers addEntriesFromDictionary:[self defaultHeaders]];
    return headers;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTP.m" line="238">

---

The function <SwmToken path="VENCore/Networking/VENHTTP.m" pos="238:5:6" line-data="- (void)setAccessToken:(NSString *)accessToken">`setAccessToken:`</SwmToken> sets the access token used for authenticated requests. It updates the internal <SwmToken path="VENCore/Networking/VENHTTP.m" pos="238:12:12" line-data="- (void)setAccessToken:(NSString *)accessToken">`accessToken`</SwmToken> property and reinitializes the <SwmToken path="VENCore/Networking/VENHTTP.m" pos="44:8:8" line-data="        self.session = [NSURLSession sessionWithConfiguration:configuration delegate:self delegateQueue:delegateQueue];">`NSURLSession`</SwmToken> with headers that include the new token.

```limbo
- (void)setAccessToken:(NSString *)accessToken
{
    _accessToken = accessToken;
    NSDictionary *headers = [self headersWithAccessToken:accessToken];
    [self initializeSessionWithHeaders:headers];
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTP.m" line="246">

---

The function <SwmToken path="VENCore/Networking/VENHTTP.m" pos="246:7:7" line-data="- (NSDictionary *)defaultHeaders">`defaultHeaders`</SwmToken> returns a dictionary of default HTTP headers used in requests. These include the <SwmToken path="VENCore/Networking/VENHTTP.m" pos="249:9:11" line-data="    [defaultHeaders addEntriesFromDictionary:@{@&quot;User-Agent&quot; : [self userAgentString],">`User-Agent`</SwmToken>, Accept, <SwmToken path="VENCore/Networking/VENHTTP.m" pos="251:3:5" line-data="                                               @&quot;Accept-Language&quot;: [self acceptLanguageString],">`Accept-Language`</SwmToken> headers, and a <SwmToken path="VENCore/Networking/VENHTTP.m" pos="252:3:5" line-data="                                               @&quot;Device-ID&quot; : [[UIDevice currentDevice] VEN_deviceIDString]}];">`Device-ID`</SwmToken> header representing the current device.

```limbo
- (NSDictionary *)defaultHeaders
{
    NSMutableDictionary *defaultHeaders = [[NSMutableDictionary alloc] init];
    [defaultHeaders addEntriesFromDictionary:@{@"User-Agent" : [self userAgentString],
                                               @"Accept": [self acceptString],
                                               @"Accept-Language": [self acceptLanguageString],
                                               @"Device-ID" : [[UIDevice currentDevice] VEN_deviceIDString]}];
    return defaultHeaders;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTP.m" line="257">

---

The function <SwmToken path="VENCore/Networking/VENHTTP.m" pos="257:7:7" line-data="- (NSString *)userAgentString">`userAgentString`</SwmToken> constructs and returns a <SwmToken path="VENCore/Networking/VENHTTP.m" pos="249:9:11" line-data="    [defaultHeaders addEntriesFromDictionary:@{@&quot;User-Agent&quot; : [self userAgentString],">`User-Agent`</SwmToken> string that identifies the app name, version, device model, <SwmToken path="VENCore/Networking/VENHTTP.m" pos="269:19:19" line-data="    NSString *userAgent = [NSString stringWithFormat:@&quot;%@/%@ (%@; iOS %@; Scale/%0.2f)&quot;, appName, appVersion, model, osVersion, [UIScreen mainScreen].scale];">`iOS`</SwmToken> version, and screen scale. It ensures the string is ASCII-encoded, transforming it if necessary.

```limbo
- (NSString *)userAgentString
{
    /**
     *  Borrowed from AFNetworking 2.5.0, with modifications.
     *  @see http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.43
     */
    NSDictionary *infoDict = [NSBundle mainBundle].infoDictionary;
    NSString *appName = infoDict[(__bridge NSString *)kCFBundleExecutableKey] ?: infoDict[(__bridge NSString *)kCFBundleIdentifierKey] ?: @"VENCore";
    NSString *appVersion = infoDict[@"CFBundleShortVersionString"] ?: infoDict[(__bridge NSString *)kCFBundleVersionKey] ?: @"0.0";
    NSString *model = [[UIDevice currentDevice] VEN_platformString];
    NSString *osVersion = [[UIDevice currentDevice] systemVersion];

    NSString *userAgent = [NSString stringWithFormat:@"%@/%@ (%@; iOS %@; Scale/%0.2f)", appName, appVersion, model, osVersion, [UIScreen mainScreen].scale];

    if (userAgent) {
        if (![userAgent canBeConvertedToEncoding:NSASCIIStringEncoding]) {
            NSMutableString *mutableUserAgent = [userAgent mutableCopy];
            if (CFStringTransform((__bridge CFMutableStringRef)(mutableUserAgent), NULL, (__bridge CFStringRef) @"Any-Latin; Latin-ASCII; [:^ASCII:] Remove", false)) {
                userAgent = mutableUserAgent;
            }
        }
    }

    return userAgent;
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTP.m" line="284">

---

The function <SwmToken path="VENCore/Networking/VENHTTP.m" pos="284:7:7" line-data="- (NSString *)acceptString">`acceptString`</SwmToken> returns the MIME type string <SwmToken path="VENCore/Networking/VENHTTP.m" pos="286:5:7" line-data="    return @&quot;application/json&quot;;">`application/json`</SwmToken> indicating that the client expects JSON responses from the server.

```limbo
- (NSString *)acceptString
{
    return @"application/json";
}
```

---

</SwmSnippet>

<SwmSnippet path="/VENCore/Networking/VENHTTP.m" line="290">

---

The function <SwmToken path="VENCore/Networking/VENHTTP.m" pos="290:7:7" line-data="- (NSString *)acceptLanguageString">`acceptLanguageString`</SwmToken> returns a string representing the current locale's language and country code in the format "language-country", which is used in the <SwmToken path="VENCore/Networking/VENHTTP.m" pos="251:3:5" line-data="                                               @&quot;Accept-Language&quot;: [self acceptLanguageString],">`Accept-Language`</SwmToken> HTTP header to indicate preferred languages.

```limbo
- (NSString *)acceptLanguageString
{
    NSLocale *locale = [NSLocale currentLocale];
    return [NSString stringWithFormat:@"%@-%@",
            [locale objectForKey:NSLocaleLanguageCode],
            [locale objectForKey:NSLocaleCountryCode]];
}
```

---

</SwmSnippet>

# Usage

## VENHTTP

VENHTTP is instantiated with a base URL to create an HTTP client configured for network requests. For example, in the payment sandbox tests, VENHTTP is initialized with the sandbox API URL and assigned to the core HTTP client to facilitate communication with the Venmo API.

In unit tests, VENHTTP is used to perform network requests with custom protocol classes for testing purposes. This allows the tests to simulate network interactions by injecting mock protocols, ensuring that the HTTP client behaves correctly under different scenarios.

Within the core implementation, VENHTTP is encapsulated inside the core class as the HTTP client, initialized during the core's setup with a specified base URL. This design centralizes network communication through VENHTTP, making it the primary interface for API requests.

Mocking VENHTTP is also demonstrated in transaction-related tests, where a mock instance replaces the real HTTP client to isolate and test transaction logic without making actual network calls. This approach improves test reliability and speed.

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBdmVubW9fVkVOQ29yZSUzQSUzQW11ZGFzaW4x" repo-name="venmo_VENCore"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
