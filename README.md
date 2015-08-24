**Guiding light**
** people first. Speed (Network usage)
** React is faster to create and send, better for users
** React is easier to reason about, great for devs

**Points to hit**
* If you have code that only run in node or only runs on the browser it's not truly universal. Share close to the entire app on both. 
* string templates are always slow
* no extra syntax to learn
* not bought into a monolithic stack
* angular sucks
* hot code swapping
* persist state on the server for debugging live users
* UI as a reflection of the data (easy debugging)
* HTML is connected and reflective of the JS. In Angular HTML is not reflective of what the javascript is doing underneath. Hard to debug.
* 2015 Developer manifesto. 
** Hot swap code, even when deep in the state of the site 
** structure code in a way that's best for developer but hyper optimize deployment for users. Allow for code to run at build time, server of on browser. 
** Testing without a browser 
** Simple UI debugging
** clear separation between view, data transforms, persistence and data fetching for easy debugging
** Reversible data transforms and UI state
** Record and send all data flow changes to the server for clear repo steps 

**Best Practice Notes**
* Always use PropTypes. Explains how to use the component.
* Render method should NEVER change anything.
* implement `componentShouldUpdate` to manually cull overrendering
* avoid `context` as much as possible
* find the highest parent possible to contain domain specific logic and keep as many components as possible to be as dumb as possible
* re-use as much of the web pack config as possible
* 

# Why React?
——

# People First 

——
### People First 
* 2,000 millisecond TTI (time to interact) decreased revenue by -4%<sup>[bing, amazon, google][page-load-stats]</sup>
* 3,000 milliseconds will lose 40% of people<sup>[yotta][bounce-rate]</sup>.
* 100 milliseconds direct manipulation. interactions makes user feel the system is reacting properly (no special feedback needed) <sup>[Jakob Nielson PhD][ui-limits]</sup>.
* 1,000 milliseconds interrupts the users flow of thought<sup>[Jakob Nielson PhD][ui-limits]</sup>.
* 10,000 milliseconds lose the user’s attention<sup>[Jakob Nielson PhD][ui-limits]</sup>.
* Amazon, Google, Bing, IFC & Chilis mobile traffic was roughly 45% of traffic.<sup>[Clickz (2015)][mobile-traffic]</sup>
* 50% of the users are on 3G and LTE users are on 3G 33% of the time.<sup>[open single (2014)][mobile-speeds]</sup>

——
### People First Metrics

* Connection speed threshold is 1.6Mbps/0.2MBps (Fastest mobile 3G speed).
* Time to Interact in less than 2,000 ms.
* Time to Transition in less than 1,000 ms.
* **400KB** is the target size for the initial page load.

——
### People First - Competitors
**TODO: film strip view of multiple sites loading for an average mobile user**

——
### People First - React in Production
**TODO: film strip load time of our landing page built in React**

——
# React Core Tenets 
* Simplify.
* **Only a view layer library.** Not a monolithic framework.
* **View is just a side-effect of data.** Or to say it another way, The view is just a reflection of the data at that snapshot. 
* Data transformation and mutation is completely decoupled from the view. 
* Components are owned by their parent with a one-way data flow.
* The view is not coupled to the painting of the screen. 

——
# React Caveats
* React manages the view layer in a way to take care of global optimizations and synchronization with the paint cycles. 
* Don’t use jQuery.
* Don’t mutate the DOM.
* React can be hard with some other libraries that use the DOM. If any library needs the DOM (like D3 or jQuery) then wrap it in a component and opt out of react’s DOM handling for that component.
* React components have the promise of Polymer’s compatibility but the React community isn’t focusing on improving that aspect of React.
* Webpack is a great build tool but a higher learning curve than Gulp/Grunt.

——
# What does this look like for developers?

——
# UI as a side effect. Immediate mode. 
* This is most critical difference with React.
* Explain possible combinations of an action with possible permutations. Each permutation has a logarithmic increase is the number of possibilities and becomes increasingly hard.
* Most UIs are a permutation of all changes since the start of the system
* The complex order of operations make it’s more brittle to build things on top of this model.
* If you've ever had to do full regressions because different parts of you app cause bugs at other ends of your app, that's because of the permutation effect. 

——
### UI as a side effect. Immediate mode

* React views are a product of the last global data snapshot against current data snapshot
* Complex permutations are able to be handled elsewhere. With UI, human interactions and time it makes it easier to isolate and unit test data transforms. 

**TODO: make example of CSS transform order of operations versus additive settings.**

——
### UI as a side effect - Hot swapping code

* Modules (and their source maps) can be replaced in the browser at runtime.
* The state of the app doesn’t need to change just the view.
* This means styles as well as interactions can be hot swapped in for live testing/debugging.

[Webpack][Webpack]
**TODO: video of webpack hot swapping code***

——
### UI as a side effect - Debugging in React
* React can snapshot the view at each change
* To debug just log the lifecycle method for new data coming in. 
* Insert image of components with which is the data-aware component and which are dumb shell components. 
* **Stop debugging call stacks** just see what the state is of the data. 
* The view shouldn’t do anything but be a reflection of the data. This allows you to **focus unit tests on data transforms** not the view. Designers always rework the view but the underlaying business rules almost never change.

——
# Data is hard 
* How do you have offline data?
* How do you synchronize data between multiple users?
* How to you reconcile offline data mutations between multiple clients?
* Remember WDSL & Soap?
* Remember XML and XPath?
* Remember JSON and REST? 
* Data retrieval, persistence and synchronization are very hard and not one solution is perfect.

——
### Data is hard - React doesn’t do data
* React is a __view__ library.
* React isn’t a monolithic framework. So you can change you’re data strategies without affecting you view code.

——
### Data - Props and State
* Props is data that has been passes to you.

```javascript
// parent
render: function() {
 return React.createElement(MyChild, {prop1:’val1’});
}
// child
render: function(){
	return React.createElement(‘div’, nil, this.props.prop1);
}
```
I
* State is data that you have control over

```javascript
// child
toggle: function(){
	this.setState({isOn: !this.state.isOn});
}
render: function(){
	var buttonProps = {onClick: function(){ this.toggle(); }};
	return React.createElement(‘button’, buttonProps, ‘Click to toggle’);
}
```

——
### Data - Props and State - One way data flow
* The highest possible parent owns the connection to the data mutations
* Parent passes immutable props to child. (in React 0.14)
* Child has internal state to hold its own record of what data is being displayed in the view.

```javascript
componentShouldUpdate: function(nextState, nextProps){
	var currentState = this.state;
	var currentProps = this.props;
}
```
* React views can change their own state but not their props.
* Use an event to send state to parent.

```javascript
// parent
increment: function(newValue){
	this.setState({prop1: newValue});
}
render: function(){
	var childProps = {prop1:1, onActivated:this.increment};
	return React.createElement(MyChild, childProps);
}
// child
render: function(){
	var buttonProps = {onClick: onClick: this.props.onActivated};
	return React.createElement(‘button’, buttonProps, ‘+’);
}
```

——
### Data - Building on Props/State with Immutable data.

**Simple made easy**

Simple - one thing  
Easy - near our capabilities  

* Immutable data is simple. It can not be changed out from under you. 
* Allows for easier concurrency and parallelism that is easier to rationalize about. 
* Immutable does not mean slow. There is an efficient diff-ing strategy. 
* Each change is just holds a pointer to a new head. All other data is shared and unchanged. 


[simple-made-easy][rich-simple]

——
### Data - Immutable data - Building on top of immutable data

* Remember a React view is just a reflection of the data at a single point in time.
* Rewind, replay and fast forward data change, which will update your view.
* Because data is immutable there is only one source of truth and it can be record simply and easily.

**TODO: Show video Reflux of rewind**  

——
### Data - Immutable data - Production debugging

* Save every diff to the server
* Restore the app state on any machine and any state just by setting up the data the same way
I
**TODO: show video of OM reviving app on another machine**

——
### Data - A possible future with Demand-Driven Architecture
* Fetch only the smallest amount of data needed to drive the view
[Demand Driven Architecture][DDA]

**TODO: Small example code of DDA**

——
# Versatile - screen painting and rendering performance 
* DOM is great for static pages but not for constantly changing app
* DOM will never be good at long scroll lists without object pooling (iOS has had this since iOS 1!)
* React targets the platforms native rendering layer. Including DOM, canvas, WebGL, iOS UIKit and soon Android Fragments. 
* Animations are smooth and easy to reason about since it's just a transition from one set of data to another set of data. 
**TODO: show responsive react site**
**TODO: get link to webGL running in React**

——
### Versatile - Network performance with Universal Javascript
* run same code on server and client-side
* need to not use browser only libraries.
** (used super agent to pull data the same on client and server)
** don’t access DOM. otherwise you’ll need to run a headless phantoms browser on the server. this will eat up a lot more CPU and run slower
* create static HTML pages, generate HTML server-side or run as a full JS app on the client with only changing a small bootstrap file

——
### Versatile - Network performance with Universal Javascript
* Example - Level Money’s infrastructure for the new web site.
* Webpack has 2 build tasks: One for the client libraries (chunked at the page level for async loading). The other is a single file   (with synchronous loading) to render on the server.
* The only difference is a single 20 line bootstrap file between the client and server code.
* The server generates a minimal set of HTML (w/ react ids).
* The HTML and CSS load first and render the view for the user.
* After the user is interacting with the content, then React and the libraries are loaded and attach __on top of__ the HTML that was already generated.
* When the user clicks on the next link it will use XHR to download the minimal JS and data to load the next page and then animate to the next page.
* If React doesn’t have time to load, links will render server-side for the next page.
* React being a side-effect of the data makes it much easier to manage this pattern.  

**TODO: change to a simple graphic**
[Webpack][Webpack]
[Webpack bootstrapping][Webpack-react-bootstrap]

——
# React - Simple made easy
* Better for developer and for end users
* Just the view layer.
* UI as a side-effect is so powerful.
* Only a few lifecycle methods to learn.
* Enable functional style programming easier.
* Simple to debug by just checking the view at any given snapshot.
* Easy to rationalize about what the system is displaying.
* Use standard JavaScript, no crazy $scope or tons of other things to learn. 
* Simple makes it possible to build more on top without adding massive layers on complexity. 

——
# Why React?

——
# Why the hell aren't you using React?

——

[page-load-stats]: http://www.guypo.com/17-statistics-to-sell-web-performance-optimization/
[ui-limits]: http://www.nngroup.com/articles/response-times-3-important-limits/
[bounce-rate]: http://www.yottaa.com/blog/application-optimization/marketing-web-performance-101-how-site-speed-impacts-your-metrics-
[web-vs-app]: http://www.smartinsights.com/mobile-marketing/mobile-marketing-analytics/mobile-marketing-statistics/
[mobile-traffic]: http://www.clickz.com/clickz/column/2388915/why-mobile-web-still-matters-in-2015
[mobile-speeds]: http://opensignal.com/reports/state-of-lte/usa-q1-2014/
[gq-faster-pages-more-people]: http://digiday.com/publishers/gq-com-cut-page-load-time-80-percent/
[rich-simple]: http://www.infoq.com/presentations/Simple-Made-Easy
[DDA]: http://www.infoq.com/presentations/domain-driven-architecture
[Webpack]: http://webpack.github.io/
[Webpack-react-bootstrap]: https://github.com/Levelmoney/generator-megatome
