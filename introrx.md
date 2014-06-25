### What is Functional Reactive Programming (FRP)?

**(You can skip this paragraph.)** There are plenty of bad explanations and definitions out there on the internet. [Wikipedia](https://en.wikipedia.org/wiki/Functional_reactive_programming) is too generic and theoretical as usual. [Stackoverflow](http://stackoverflow.com/questions/1028250/what-is-functional-reactive-programming)'s canonical answer is obviously not suitable for new comers. [Reactive Manifesto](http://www.reactivemanifesto.org/) sounds like the kind of thing you show to your project manager or the businessmen at your company. Microsoft's Rx terminology "Rx = Observables + LINQ + Schedulers" is so heavy and Microsoftish that most of us are left confused. Terms like "reactive" and "propagation of change" don't convey anything specifically different to what your typical MV* and favorite language already does. Of course my framework views react to the models. Of course change is propagated. If it wouldn't, nothing would be rendered.

So let's cut the bullshit. 

###FRP is programming with asynchronous data (or event) streams. 

In a way, this isn't anything new. Your typical click events are really an asynchronous event stream, on which you can observe and do some side effects. FRP takes that idea and puts it on steroids. You are able to create data streams of anything, not just from click and hover events. Imagine your Twitter feed would be a data stream in the same fashion that click events are. So you can listen to that stream and react accordingly.

On top of that, you are given a large toolbox of functions to combine, create and filter any of those streams. That's where "functional" kicks in. A stream can be used as an input to another one. Even multiple streams can be used as inputs to another stream.

```coffeescript
allSuggestions = 

followSuggestions = ...

```

http://jsfiddle.net/staltz/8jFJH/7