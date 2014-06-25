### What is Functional Reactive Programming (FRP)?

**(You can skip this paragraph.)** There are plenty of bad explanations and definitions out there on the internet. [Wikipedia](https://en.wikipedia.org/wiki/Functional_reactive_programming) is too generic and theoretical as usual. [Stackoverflow](http://stackoverflow.com/questions/1028250/what-is-functional-reactive-programming)'s canonical answer is obviously not suitable for new comers. [Reactive Manifesto](http://www.reactivemanifesto.org/) sounds like the kind of thing you show to your project manager or the businessmen at your company. Microsoft's Rx terminology "Rx = Observables + LINQ + Schedulers" is so heavy and Microsoftish that most of us are left confused. Terms like "reactive" and "propagation of change" don't convey anything specifically different to what your typical MV* and favorite language already does. Of course my framework views react to the models. Of course change is propagated. If it wouldn't, nothing would be rendered.

So let's cut the bullshit. 

#### FRP is programming with asynchronous data (or event) streams. 

In a way, this isn't anything new. Your typical click events are really an asynchronous event stream, on which you can observe and do some side effects. FRP takes that idea and puts it on steroids. You are able to create data streams of anything, not just from click and hover events. Anything can be a stream: variables, user inputs, caches, data structures, etc. E.g., imagine your Twitter feed would be a data stream in the same fashion that click events are. So you can listen to that stream and react accordingly.

On top of that, you are given an amazing toolbox of functions to combine, create and filter any of those streams. That's where "functional" kicks in. A stream can be used as an input to another one. Even multiple streams can be used as inputs to another stream. You can _merge_ two streams. You can _filter_ a stream to get another one that has only those events you are interested in. You can _map_ data values from one stream to another new one.

If streams are so central to FRP, let's take a careful look at them, starting from our familiar "clicks on a button" event stream.

![Click event stream](https://gist.githubusercontent.com/staltz/868e7e9bc2a7b8c1f754/raw/733a91fd36cb655a7e706d1ac9caab6f788b9a1d/zclickstream.png)

A stream is a sequence of ongoing events ordered in time. It can emit three different things: a value (of some type), an error, or a "completed" signal. Consider that the "completed" takes place, for instance, when the current window or view containing that button is closed.



```coffeescript
allSuggestions = 

followSuggestions = ...

```

http://jsfiddle.net/staltz/8jFJH/7