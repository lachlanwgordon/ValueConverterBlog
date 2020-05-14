# Using Value Converters To Clean Up Your Xamarin.Forms MVVM Bindings
## Basics of why you would want a value converter
When working with DataBinding in Xamarin.Forms you can often end up with a property in your ViewModel that's of the wrong type or needs to be converted. 

For example if you've got an ActivityIndicator that you bind to a boolean property `IsBusy` but you want another element to only appear when not busy. An obvious approach to take it to use an other propery called `IsNotBusy` but then you've got the data in two spots that you need to worry about updating and making sure you `NotifyPropertyChanged` for both. This is also not very reusable if you want to bind to the inverse value of a boolean multiple places in your app.

This is where a value converter comes in handy. When we create an `InverseBoolConverter` that we can use anywhere throughout the app.

Other useful scenarios for using a value converter include:
* Changing the text displayed on a button based on the current state of an item
* Highlighting an area in red if it is a negative number


## Example
 
 
 
 
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
 
 A lot of this boiler plate can be skipped by having MFractor generate the converter for you using the wizard or from a quick fix in a xaml file.
 
 To access the Value Converter Wizard, open the `MFractor` Menu, then `Wizards`, then `Value Converter Wizard`. In many cases all you will need to do here is give your converter a name and click `Generate Value Converter` and you'll be good to go. You'll have a the methods half way implemented with type checking and casting all wired up and only your logic left to implement. It will also declare the value converter as a `StaticResource` so you can use it in a binding straight away.
 
 If you've followed the naming convention of `{Type}To{Type}Converter` MFractor can infer the types you're working with the so the type checks can be handled for you. If you've named your converter differently or you disable `Infer Input/Output Types` you can stil manually specify the types. Along with the input/output types you can also specify a parameter type if required to get you one step closer to a complete converter.
 
 The final preference lets you chose where you want the `StaticResource` to be declared. If you have a xaml file open when you run the wizard you'll have the option of of adding it to the page's resources, otherwise you can add it to your App.xaml for access from all pages.
 
 
 
## Generate from XAML quick fix
 * how to use
 * GIF of gen in action
 
 The wizard is great but if your working away on a xaml file and you suddenly discover your need a converter, you can also generate a converter from the right click menu, along side all the other MFractor refactorings and generations and the built in Visual Studio quick fixes. The action from the right click menu gives you all the same benefits as accessing the wizard from the menu, with the added benefit of setting the value converter on your binding.
 
 
 
## Benefits for MVVM 
ValueConverters don't require you to follow an MVVM patern but the do work really nicely together and help to keep your view models clean and easier to read.

Some of the benefits for your view models include:
 * No colors in your view models. Their presence here is up for debate but if you want to cut down on how much `V` is in your `VM` these can be some of the hardest parts to separate out without ValueConverters
 * remove getters for derived fields
 * Remove complex getters setters where values are linked e.g. IsBusy/IsNotBusy
 * Reduce calls to OnPropertyChanged() by allowing you to bind multiple view properties to one view model property.
 
## Advanced
 * Parameter(snippet)
 
 Sometimes a value converter needs input from the view to provide context, this is where the `ValueConverterParameter` comes in handy.
 
 
 
 * Attribute(snippet)
 
## Summary
Value converters are one of those neat features in Xamarin.Forms where once you start using them, you won't know how you lived without them. Having your type conversion logic in one reusable place lets you focus on the code that matters in your view models without as much noise. Their main drawback is that their is a fair bit of boilerplate to get started but MFractor does all the heavy lifting for you with just a little prompting.
