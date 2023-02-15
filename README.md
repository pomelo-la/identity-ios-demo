

# Getting started

* [System Requirements](#system-requirements)
* [Adding Identity](#adding-identity)
  1. Import dependency
  2. Register SDK into your Application
  3. Authorization
* [Setting up your theme](#setting-up-your-theme)
* [Release Notes](#release-notes)

# System Requirements

**Minimum iOS target:** 13

# Adding Identity

## 1. Import dependency

### SPM

To install the SDK via SPM, follow these steps:

1. In Xcode, select File > Swift Packages > Add Package Dependency and enter https://github.com/pomelola/identity-ios-sdk.git as the repository URL.
2. Select a minimum version of 1.0.0 (latest version available in release notes)

### Cocoapods

1. Add our private spec repository to your CI/Local environment:

``` ruby
pod repo add pomelo-specs https://github.com/pomelo/Specs.git
```

2. Add our repo source at the top of your Podfile:

``` ruby
source 'https://github.com/pomelo/Specs.git'
```

3. Last, just add the dependency to your app’s target.

``` ruby
target 'MyApp' do
  pod 'PomeloIdentity', '~> 1.0.0'
end
```

## 2. Initialize Identity

In order to initialize the SDK, you have to setup the initial configuration:
* **Environment**: An enum indicating the environment that can be development or production. The available values are: ```.development``` or ```.production```
* **SessionID**: Is a String that indicates the identity session id to be instantiated.

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
### Result 
We provide a set of callbacks to you, in order to give you feedback about the session status. The different results are the following:

* **Success**: This state indicates that the flow was finished successfully meaning that the session could be pending (data is being processed), in manual review or fully validated. 
* **Expired**: This state indicates that the session is expired meaning that is no longer available and the client should generate a new one.
* **Cancel by user**: This state indicates that the user has canceled the session, it is possible to resume the session if it is not expired.
* **Rejected**: indicates that session is rejected meaning that user is no longer eligible for identity check.
* **Not validated**: indicates that the information provided by the user is not valid but can start the process again with a new session id.

## 3. Authorization
To communicate with Pomelo's API you must generate a **Client Token** to create/get an **User ID** and generate a **Session ID**. Usually this bussiness logic should live in your backend.

To communicate with Pomelo's Identity SDK you must generate an **End User Token** which is a JWT token that expires in a certain amount of time and it has to be generated again and set in the `AuthTokenListener`

<img src="https://user-images.githubusercontent.com/9848247/187688179-4a1b44c3-e1a8-4692-b00d-581e6f672559.png"/>

*Click image to view full screen*

&nbsp;

# Setting up your theme

This step is necessary for your app style to our SDK. You can customize colors, buttons, feedback screens and fonts.

``` swift
public  struct  MyAppTheme: PomeloThemeable {
    public  var colors: PomeloColors
    public  var buttons: PomeloButtonsStyle
    public  var feedbacks: PomeloFeedback?
    public  var font: UIFont?
}
```

* Colors: allows client to change primary, secondary and background color. 
* Buttons: allows client to change primary, secondary, tertiary and link buttons style.
* Font: allows client to change family font. Take notice that weight and size will be determined by the screen and cannot be changed.
* Feedbacks: allows client to define specific background to feedback screens and custom icons.

As shown above, you have to conform to PomeloThemeable protocol. Later on, you have to initialize the struct with your custom configuration like so:

``` swift
let myTheme = PomeloTheme(
    colors: .init(primary: .primaryColor,
                  secondary: .secondaryColor,
                  background: .primaryColor),
    buttons: .init(primary: primaryButton,
                   secondary: secondaryButton,
                   tertiary: tertiaryButton,
                   link: linkButton),
    feedbacks: .init(
        icons: [
            .error: UIImage(named: String.Image.failureIcon, in: PomeloUIBundle.resourceBundle, compatibleWith: nil)!,
            .info: UIImage(named: String.Image.infoIcon, in: PomeloUIBundle.resourceBundle, compatibleWith: nil)!,
            .waiting: UIImage(named: String.Image.waitingIcon, in: PomeloUIBundle.resourceBundle, compatibleWith: nil)!,
            .success: UIImage(named: String.Image.successIcon, in: PomeloUIBundle.resourceBundle, compatibleWith: nil)!,
            .warning: UIImage(named: String.Image.warningIcon, in: PomeloUIBundle.resourceBundle, compatibleWith: nil)!],
        textColor: .black),
    font: UIFont(name: “Verdana”, size: 10)
)
```
*Note: UIFont size will be ignored, size will be determined by the UI components used in each screen.*



# Release Notes
## 1.0.0
##### Date 31/12/2022
### Overview
### Fixes
### Improvements
