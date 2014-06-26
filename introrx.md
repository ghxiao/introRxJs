## The introduction to Reactive Programming you've been missing

You probably opened this tutorial because you're curious in learning this new thing called Reactive Programming.

Learning (Functional) Reactive Programming is hard, even harder by the lack of good material. When I started learning, I tried looking for tutorials. I found only a handful of practical guides, but they only scratched the surface and never tackled the challenge of building the whole architecture around FRP. Library documentations often don't help when you're trying to understand some function. I mean, honestly, look at this:

> **Rx.Observable.prototype.flatMapLatest(selector, [thisArg])**

> Projects each element of an observable sequence into a new sequence of observable sequences by incorporating the element's index and then transforms an observable sequence of observable sequences into an observable sequence producing values only from the most recent observable sequence.

Holy cow.

I've read two books, one just painted the big picture, while the other dived into how to use the FRP library. I ended up learning Reactive Programming the hard way: figuring it out while building with it. At my work in [Futurice](https://www.futurice.com) I got to use it in a real project, and had the [support of some colleagues](http://blog.futurice.com/top-7-tips-for-rxjava-on-android) when I ran into troubles.

The hardest part of the learning journey is **thinking in FRP**. It's a lot about letting go of old imperative and stateful habits of typical programming, and forcing your brain to work in a different paradigm. I haven't found any guide on the internet in this aspect, and I think the world deserves a practical tutorial on how to think in FRP, so that you can get started. Library documentation can light your way after that. I hope this helps you.

### "What is Functional Reactive Programming (FRP)?"

(You can skip this paragraph.) There are plenty of bad explanations and definitions out there on the internet. [Wikipedia](https://en.wikipedia.org/wiki/Functional_reactive_programming) is too generic and theoretical as usual. [Stackoverflow](http://stackoverflow.com/questions/1028250/what-is-functional-reactive-programming)'s canonical answer is obviously not suitable for new comers. [Reactive Manifesto](http://www.reactivemanifesto.org/) sounds like the kind of thing you show to your project manager or the businessmen at your company. Microsoft's [Rx terminology](https://rx.codeplex.com/) "Rx = Observables + LINQ + Schedulers" is so heavy and Microsoftish that most of us are left confused. Terms like "reactive" and "propagation of change" don't convey anything specifically different to what your typical MV* and favorite language already does. Of course my framework views react to the models. Of course change is propagated. If it wouldn't, nothing would be rendered.

So let's cut the bullshit. 

#### FRP is programming with asynchronous event streams. 

In a way, this isn't anything new. Your typical click events are really an asynchronous event stream, on which you can observe and do some side effects. FRP takes that idea and puts it on steroids. You are able to create data streams of anything, not just from click and hover events. Streams are cheap and ubiquitous, anything can be a stream: variables, user inputs, properties, caches, data structures, etc. For example, imagine your Twitter feed would be a data stream in the same fashion that click events are. You can listen to that stream and react accordingly.

**On top of that, you are given an amazing toolbox of functions to combine, create and filter any of those streams.** That's where the "functional" magic kicks in. A stream can be used as an input to another one. Even multiple streams can be used as inputs to another stream. You can _merge_ two streams. You can _filter_ a stream to get another one that has only those events you are interested in. You can _map_ data values from one stream to another new one.

If streams are so central to FRP, let's take a careful look at them, starting with our familiar "clicks on a button" event stream.

![Click event stream](https://gist.githubusercontent.com/staltz/868e7e9bc2a7b8c1f754/raw/49da694b2489f9e7b7276df31a1dcb206179a496/zclickstream.png)

A stream is a sequence of **ongoing events ordered in time**. It can emit three different things: a value (of some type), an error, or a "completed" signal. Consider that the "completed" takes place, for instance, when the current window or view containing that button is closed.

We capture these emitted events only **asynchronously**, by defining a function that will execute when a value is emitted, another function when an error is emitted, and another function when 'completed' is emitted. Sometimes these last two can be omitted and you can just focus on defining the function for values. The "listening" to the stream is called **subscribing**. The functions we are defining are observers. This is precisely the [Observer Design Pattern](https://en.wikipedia.org/wiki/Observer_pattern).

Since this feels so familiar already, and I don't want you to get bored, let's do something new: we are going to create new click event streams transformed out of the original click event stream.

Let's just say that you want to have a stream of "double click" events. To make it even more interesting, let's say we want the new stream to consider triple clicks as double clicks, or in general, multiple clicks (two or more). Take a deep breath and imagine how you would do that in a traditional imperative and stateful fashion. I bet it sounds fairly nasty and involves some variables to keep state and some fiddling with time intervals.

Well, in FRP it's pretty simple. In fact, the logic is just 4 lines of code. http://jsfiddle.net/staltz/4gGgs/24/
But let's ignore code for now. Thinking in diagrams is the best way to understand and build streams, whether you're a beginner or an expert.

![Multiple clicks stream](https://gist.githubusercontent.com/staltz/868e7e9bc2a7b8c1f754/raw/7ddf577a8e1ea0b03471cd885070114dff3a74b2/zmulticlickstream.png)

Grey boxes are functions transforming one stream into another. First we accumulate clicks in lists, whenever 250 milliseconds of "event silence" has happened (that's what `buffer(stream.throttle(250ms))` does, in a nutshell). The result is a stream of lists, from which we apply `map()` to map each list to an integer matching the length of that list. Finally, we ignore `1` integers using the `filter(x >= 2)` function. That's it: 3 operations to produce our intended stream. We can then subscribe ("listen") to it to react accordingly how we wish. 

I hope you enjoy the beauty of this approach. This example is just the tip of the iceberg: you can apply the same operations on different kinds of streams, for instance, on a stream of API responses; on the other hand, there are many other functions available.

### "Why should I consider adopting FRP?"

FRP raises the level of abstraction of your code so you can focus on the interdependence of events that define the business logic, rather than having to constantly fiddle with a large amount of implementation details. Code with FRP will likely be more concise.

The benefit is more evident in modern webapps and mobile apps, that are highly interactive with a multitude of UI events related to data events. 10 years ago, interaction with web pages was basically about submitting a long form to the backend and performing simple rendering to the frontend. Apps have evolved to be more real-time: modifying a single form field can automatically trigger a save to the backend, "likes" to some content can be reflected in real time to other connected users, and so forth.

Apps nowadays have an abundancy of real-time events of every kind that enable a highly interactive experience to the user. We need tools for properly dealing with that, and Functional Reactive Programming is the answer.

### Thinking in FRP, with examples

Let's dive into the real stuff. A real-world example with a step-by-step guide on how to think in FRP. No synthetic examples, no half-explained concepts. By the end of this tutorial we will have produced real functioning code, while knowing why we did each thing.

I picked **Javascript** and **RxJS** as the tools for this, for a reason: Javascript is the most familiar language out there at the moment, and the Rx* library family is widely available for many languages (C#, Java, Javascript, Ruby, Python, C++, etc). So whatever your tools are, you can concretely benefit by following this tutorial.

### Implementing a "Who to follow" suggestions box in FRP

In Twitter there's is this UI element that suggests other accounts you could follow:

![Twitter Who to follow suggestions box](https://gist.githubusercontent.com/staltz/868e7e9bc2a7b8c1f754/raw/eb151a86fb9f6496937b9bd9758c3d4970a2e2e1/ztwitterbox.png)

http://jsfiddle.net/staltz/8jFJH/33