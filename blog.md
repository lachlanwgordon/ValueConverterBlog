# Using Value Converters To Clean Up Your Xamarin.Forms MVVM Bindings
## Basics of why you would want a value converter
When working with DataBinding in Xamarin.Forms you can often end up with a property in your ViewModel that's of the wrong type or needs to be converted. 

For example if you've got an ActivityIndicator that you bind to a boolean property `IsBusy` but you want another element to only appear when not busy. An obvious approach to take it to use an other propery called `IsNotBusy` but then you've got the data in two spots that you need to worry about updating and making sure you `NotifyPropertyChanged` for both. This is also not very reusable if you want to bind to the inverse value of a boolean multiple places in your app.

This is where a value converter comes in handy. When we create an `InverseBoolConverter` that we can use anywhere throughout the app.

Other useful scenarios for using a value converter include:
* Changing the text displayed on a button based on the current state of an item
* Highlighting an area in red if it is a negative number

 
## How to use a value converter
Using a value converter typically involves three steps
1. Writing a value converter
2. Declaring it as a static resource
3. Consuming it in your views.

Let's say we've got a `double` property in our view model `Total`.
```
public double Total {get;set;}
```

We  want to display the field in Red if it is negative, Yellow if exactly 0 and white if positing. We'll solve this with a converter called `DoubleToColourConverter`. The name doesn't really matter but people often follow the convention of having the types in the name.

Start by making a class for your value converter and have it implement `IValueConverter`. You'll end up with a skeleton class that looks like this:

```
public class DoubleToColourConverter : IValueConverter
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

We've got one method that converts a value and another that converts it back. In our case we'll want to return a `Color` based on the input number value which all happens in the Convert() method. Which will have an input parameter as a double and we should return a `Color`. Unfortunately this method isn't type safe so we'll have to do a couple of type checks, and then asuming it is a double, return the appropriate color.

Which ends up looking like this:

```
public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
{
    //Check if the value is a bool, if it is asign it to the variable input
    if (!(value is double input))
    {
        //This only happens if we try to use this converter on a property in our view model of a different type.
        return default(Color);
    }

    //We can now safely perform calculations with our `input`
    
    if(input < 0)
        return Color.Red;
    else if(input == 0)
        return Color.Yellow;
    else
        return Color.Black;
}
```

In our case the ConvertBack() method will never be called because it only really makes sense in a one way binding context.

The next step is to add the converter as a `StaticResource` so that our xaml properties know where to find it. Just like any `StaticResource` you have the choice of adding it to your App.xaml if you plan to use this resource on multiple pages or in the page's resource dictionary if you don't plan to use it elsewhere. Let's just add it to one page for now wh

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
