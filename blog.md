# Plan
## Basics of why you would want a value converter
When working with DataBinding in Xamarin.Forms you can often end up with a property in your ViewModel that's of the wrong type or needs to be converted. 

For example if you've got an ActivityIndicator that you bind to a boolean property `IsBusy` but you want another element to only appear when not busy. An obvious approach to take it to use an other propery called `IsNotBusy` but then you've got the data in two spots that you need to worry about updating and making sure you NotifyPropertyChanged for both. This is also not very reusable if you want to bind to the inverse value of a boolean multiple places in your app.

This is where a value converter comes in handy. When we create an `InverseBoolConverter` that we can use anywhere throughout the app.

Other useful scenarios for using a 

 * Concept of a value converter
 * Common Scenarios(notBool,)
 
## How to use a value converter
Using a value converter typically involves three steps
1. Writing a value converter
2. Declaring it as a static resource
3. Consuming it in your views.

Let's say we've got a boolean property in our view model `IsBusy` and we  want to display an activity indicator when the property is true but when it's false our button is enabled. We'll solve this with a converter called `InverseBoolConverter`. This is probably the most commonly used converter, some people call it a `NotBoolConverter` – the name doesn't really matter, as long as it makes sense to you.

Start by making a class for your value converter and have it implement IValueConvert. You'll end up with a skeleton class that looks like this:

```
public class InverseBoolConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
    ...
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
    ...
    }
}
```

You've got one method that converts a value and another that converts it back. In our case all we want to be able to do is convert false to true and true to false which all happens in the Convert() method. Which will have an input parameter as a bool and should return a bool. Unfortunately this method isn't type safe so we'll have to do a type check, and then asuming it is a bool, return the opposite.

Which ends up looking like this:

```
public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
{
    //Check if the value is a bool, if it is asign it to the variable input
    if (!(value is bool input))
    {
        //This only happens if we accidentally bind a bool property to a value that is a different type.
        return default(bool);
    }

    //Return the opposite of input
    return !input;
}
```

In our case the ConvertBack() method will never be called because we're only doing a one way binding, this is quite common but just to be safe we might as well write the other half now incase we need it later one. For a simple inverse bool converter both methods look exactly the same.
```
public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
{
    if (!(value is bool input))
    {
        return default(bool);
    }

    return !input;
}
```

 * How to write one by hand with no MFractor
 * Code Snippets with gif of result on phone
## The MFractor Wizard
 * How to use
 * GIF of wizard in action
 
## Generate from XAML quick fix
 * how to use
 * GIF of gen in action
 
## Benefits for MVVM 
 * No colors in VM
 * remove getters for derived fields
 * Remove complex getters setters for IsBusy/IsNotBusy
 
## Advanced
 * Parameter(snippet)
 * Attribute(snippet)
## Summary
