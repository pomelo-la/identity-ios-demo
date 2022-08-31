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
