
# Getting started

* [System Requirements](#system-requirements)

* [Using Identity](#using-identity)
  1. Install the SDK
  2. Initialize the SDK
  3. Authorization
* [Setting up your theme](https://github.com/pomelo-la/ui-ios/blob/develop/README.md)
* [Release Notes](#release-notes)

# System Requirements

**Minimum iOS target:** 13

# Using Identity

## Install the SDK

### SPM

To install the SDK via SPM, follow these steps:

1. In Xcode, select File > Swift Packages > Add Package Dependency and enter https://github.com/pomelola/identity-ios-sdk.git as the repository URL.
2. Select a minimum version of 1.0.0 (latest version available in release notes)

### Cocoapods

1. Add our private spec repository to your CI/Local environment:

```
pod repo add pomelo-specs https://github.com/pomelo/Specs.git
```

2. Add our repo source at the top of your Podfile:

```
source 'https://github.com/pomelo/Specs.git'
```

3. Last, just add the dependency to your app’s target.

```
target 'MyApp' do
  pod 'PomeloIdentity', '~> 1.0.0'
end
```

## Initialize the SDK 

In order to initialize the SDK, you need to indicate the initial configuration:
* **Environment**: development or production
* **SessionID**: the identity session id to be instantiated.

``` swift
let configuration = PomeloIdentityConfiguration(environment: .development, sessionId: "iss-usersessionid")

PomeloIdentitySDK.shared.initialize(with: configuration)
```

Later on, you should call the launch method with a UINavigationController. Identity SDK will use that controller during the flow:

``` swift
PomeloIdentitySDK.shared.launchIdentity(on: navigationController) { result in
     switch result {
     case .success:
        // information for identity check was provided successfully.
     case .failure(let error):
          switch error {
          case .expired, .cancelByUser, .rejected, .notValidated:
             // handle in case of error
          }
}
```

A set of callbacks will be provided to the client in order to provide feedback about the status of the session:

* **Success**: This state indicates that the flow was finished successfully meaning that the session could be pending (data is being processed), in manual review or fully validated. 
* **Expired**: This state indicates that the session is expired meaning that is no longer available and the client should generate a new one.
* **Cancel by user**: This state indicates that the user has canceled the session, it is possible to resume the session if it is not expired.
* **Rejected**: indicates that session is rejected meaning that user is no longer eligible for identity check.
* **Not validated**: indicates that the information provided by the user is not valid but can start the process again with a new session id.

## Authorization
To communicate with Pomelo's API you must [generate an Access Token](https://developers.pomelo.la/api-reference/general/autorizacion/solicitar-token) to create/get an **User ID** and generate a **Session ID**. Usually this bussiness logic should live in your backend.

To communicate with Pomelo's Identity SDK you must generate an **End User Token** which is a JWT token that expires in a certain amount of time and it has to be generated again and set in the `AuthTokenListener`

<img src="https://user-images.githubusercontent.com/9848247/187751184-6aa86f71-0941-4dc6-876b-6dd717ceca43.png"/>

*Click image to view full screen*


# Release Notes
## 1.0.0
##### Date 31/12/2022
### Overview
### Fixes
### Improvements
