### What is Functional Reactive Programming (FRP)?

**(You can skip this paragraph.)** There are plenty of bad explanations and definitions out there on the internet. [Wikipedia](https://en.wikipedia.org/wiki/Functional_reactive_programming) is too generic and theoretical as usual. [Stackoverflow](http://stackoverflow.com/questions/1028250/what-is-functional-reactive-programming)'s canonical answer is obviously not suitable for new comers. [Reactive Manifesto](http://www.reactivemanifesto.org/) sounds like the kind of thing you show to your project manager or the businessmen at your company. Microsoft's [Rx terminology](https://rx.codeplex.com/) "Rx = Observables + LINQ + Schedulers" is so heavy and Microsoftish that most of us are left confused. Terms like "reactive" and "propagation of change" don't convey anything specifically different to what your typical MV* and favorite language already does. Of course my framework views react to the models. Of course change is propagated. If it wouldn't, nothing would be rendered.

So let's cut the bullshit. 

#### FRP is programming with asynchronous data (or event) streams. 

In a way, this isn't anything new. Your typical click events are really an asynchronous event stream, on which you can observe and do some side effects. FRP takes that idea and puts it on steroids. You are able to create data streams of anything, not just from click and hover events. Anything can be a stream: variables, user inputs, caches, data structures, etc. For example, imagine your Twitter feed would be a data stream in the same fashion that click events are. You can listen to that stream and react accordingly.

On top of that, you are given an amazing toolbox of functions to combine, create and filter any of those streams. That's where "functional" kicks in. A stream can be used as an input to another one. Even multiple streams can be used as inputs to another stream. You can _merge_ two streams. You can _filter_ a stream to get another one that has only those events you are interested in. You can _map_ data values from one stream to another new one.

If streams are so central to FRP, let's take a careful look at them, starting from our familiar "clicks on a button" event stream.

![Click event stream](https://gist.githubusercontent.com/staltz/868e7e9bc2a7b8c1f754/raw/733a91fd36cb655a7e706d1ac9caab6f788b9a1d/zclickstream.png)

A stream is a sequence of **ongoing events ordered in time**. It can emit three different things: a value (of some type), an error, or a "completed" signal. Consider that the "completed" takes place, for instance, when the current window or view containing that button is closed.

We capture these emitted events only **asynchronously**, by defining a function that will execute when a value is emitted, another function when an error is emitted, and another function when 'completed' is emitted. Sometimes these last two can be omitted and you can just focus on defining the function for values. The "listening" to the stream is called **subscribing**. The functions we are defining are observers. This is precisely the [Observer Design Pattern](https://en.wikipedia.org/wiki/Observer_pattern).

Since this feels so familiar already, and I don't want you to get bored, let's do something new: we are going to create new click event streams transformed out of the original click event stream.

Let's just say that you want to have a stream of "double click" events. To make it even more interesting, let's say we want the new stream to consider triple clicks as double clicks, or in general, multiple `x` clicks, where `x >= 2`. Take a deep breath and imagine how you would do that in a traditional imperative and stateful fashion. I bet it sounds fairly nasty and involves some variables to keep state and some fiddling with time intervals.

Well, in FRP it's pretty simple. In fact, the logic is just 3 lines of code. http://jsfiddle.net/staltz/4gGgs/20/



http://jsfiddle.net/staltz/8jFJH/33