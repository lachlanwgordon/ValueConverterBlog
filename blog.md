# Plan
## Basics of why you would want a value converter
When working with DataBinding in Xamarin.Forms you can often end up with a property in your ViewModel that's of the wrong type or needs to be converted. 

For example if you've got an ActivityIndicator that you bind to a boolean property `IsBusy` but you want another element to only appear when not busy. An obvious approach to take it to use an other propery called `IsNotBusy` but then you've got the data in two spots that you need to worry about updating and making sure you NotifyPropertyChanged for both. This is also not very reusable if you want to bind to the inverse value of a boolean multiple places in your app.

This is where a value converter comes in handy. When we create an `InverseBoolConverter` that we can use anywhere throughout the app.

Other useful scenarios for using a 




 * Concept of a value converter
 * Common Scenarios(notBool,)
 
## How to use a value converter
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
