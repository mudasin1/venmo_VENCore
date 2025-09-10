## VENCore

## Users
* Venmo app / VenmoClient
* VENAppSwitchSDK
* VenmoSDK
* Venmo

## Compatibility
* Mac OS X 10.?
* iOS 6+
* ARC Only

## Class hierarchy
### Public interface
* `VENCore` - public facing interface
  * Responsible for:
    * Providing a simple interface for interacting with the Venmo internal API

### Models
* Should we have type-safe representations of Venmo resources? (YES)

### Service
* `VENClientAPI` – translates raw HTTP responses into domain objects
  * Responsible for:
    * API endpoint names
    * access tokens
    * default headers
    * response parsing
