# Simple, Immediate. 

—

# Simple, Immediate. 

* Simple - do one thing.
* **Im**-mediate - in
* Im-**mediate** - now

<p class=“notes”>
Being simple is hard. We conflict multiple pieces without realizing. 
Immediate means we demission the effect of time as much as possible. Doing things in the now means direct. Not history of past and no supposition of the future.
</p>

—

# Real world examples.

<p class=“notes”>
Show a example of a real world app. What are the permutations that happen in the front-end, session storage, back-end server and third party services’ servers.
</p>

—

# Real World Task - edit turning accounts on/off
![](./reactables-bank_cards.png)

<p class=“notes”>
Show the real world use case of multiple states. Each reflects front-end, back-end and 3rd party servers getting into different states and synchronizing the status in real-time.
Meaning not simple. Multiple things are happening and conflating into the UI.
This means not immediate. There needs to be a historical path of changes to login, send bank connect password to get into a set state.
</p>

—

# Real World - Flow
![](./reactables-flow.jpg)
<p class=“notes”>
This is a simple version of how the user gets into this state in the front-end app. As you can see there’s a lot of step to get to the single page to connect a bank account. Once on that page there are even more states combinations with multiple services to get into the proper UI.
</p>

—

# Real World - Task Flow

<div class=“display:flex; justify-content: stretch;”>
<div class=“height: 100%; flex-basis: 50%”>

- Registration Page
- render registration template
- event - enter username/password
- event - click Register
- validateEmail
- validatePasswordSecurity
- network - send {email:””,password:””}
- network - approval receive {token:””}
- navigateTo(“/selection”)
- render selection template
- event - click bank button
- set {banks:[{id:1, name:”bank 1”, hasPin:false}]}
- event - click next button
- navigateTo(“/verification”)
- render verification page
- event - enter username/password
- event - click connect
- network - send {username:””,password:””,bankId:””}
- receive {challengeQuestions:[“”], sessionId:””}
- render verify-challenge template
- event - enter challenge answer
- event - click answer
- network - send {sessionId:””, answers:[]}
- receive {status:”connected”, accounts:[{name:””,id:””,active:true},…]
- render account template
- finally in a state to test

</div>
<div class=“height: 100%; flex-basis: 50%”>
![](./reactables-accounts-connected.png)
</div>
</div>
<p class=“notes”>
These are the steps the front-end needs to take to get to the same place. It looks complex because it is. It’s not simple. Why?
</p>

—

# Permutations

- **per**-mutation - for each
- per-**mutation** - mean change
- Each item and the __order__ of those items, trigger a different change
- very complex
- easy to break
- Easy for a bug at one end to affect another end

<p class=“notes”>
Thinking back to stats class, what is a permutation? Each item increases the possible out commons dramatically. Just 4 changes in the system have 24 possible permutations. 
</p>

--

# Permutations - Why are they bad? 

<p class=“notes”>
Permutations are the reason for most of our complexities. Each mutation and the order of each <b>infect</b> our apps. JS is single threaded to reduce external factors but there are still multiple factors involved: network loading, server response times, 3rd party server responses, user actions, UX designer intent, …
</p>

--

# Permutations - Conflating concerns

<div class=“display:flex; justify-content: stretch;”>
<div class=“height: 100%; flex-basis: 50%”>

- **user interaction** Registration Page
- **template render** render registration template
- **user interaction** event - enter username/password
- **user interaction** event - click Register
- **app function** validateEmail
- **app function** validatePasswordSecurity
- **network & back-end** network - send {email:””,password:””}
- **data** network - approval receive {token:””}
- **app state** navigateTo(“/selection”)
- **template rendering** render selection template
- **user interaction** event - click bank button
- **data** set {banks:[{id:1, name:”bank 1”, hasPin:false}]}
- **user interaction** event - click next button
- **app state** navigateTo(“/verification”)
- **template render** render verification page
- **user interaction** event - enter username/password
- **user interaction** event - click connect
- **3rd party service & network** network - send {username:””,password:””,bankId:””}
- **3rd party service** receive {challengeQuestions:[“”], sessionId:””}
- **view state** render verify-challenge template
- **user interaction** event - enter challenge answer
- **user interaction** event - click answer
- **3rd party service** network - send {sessionId:””, answers:[]}
- **data** receive {status:”connected”, accounts:[{name:””,id:””,active:true},…]
- **view state** render account template
- **view state** finally in a state to test

</div>
<div class=“height: 100%; flex-basis: 50%”>
![](./reactables-accounts-connected.png)
</div>
</div>

<p class=“notes”>
Let’s categorize each task by it’s concern. These don’t even include animations or decisions made by the UX designer.
</p>

—

# Simplify

### What can we distill down to the essence of what is needed?

<p class=“notes>
So can we simplify? Can we be immediate? Can we reduce the complexity by isolating the externalities? 
</p>

--

# Simplify

<div class=“display:flex; justify-content: stretch;”>
<div class=“height: 100%; flex-basis: 50%”>

- **user interaction** Registration Page
- **template render** render registration template
- **user interaction** event - enter username/password
- **user interaction** event - click Register
- **app function** validateEmail
- **app function** validatePasswordSecurity
- **network & back-end** network - send {email:””,password:””}
- **data** network - approval receive {token:””}
- **app state** navigateTo(“/selection”)
- **template rendering** render selection template
- **user interaction** event - click bank button
- **data** set {banks:[{id:1, name:”bank 1”, hasPin:false}]}
- **user interaction** event - click next button
- **app state** navigateTo(“/verification”)
- **template render** render verification page
- **user interaction** event - enter username/password
- **user interaction** event - click connect
- **3rd party service & network** network - send {username:””,password:””,bankId:””}
- **3rd party service** receive {challengeQuestions:[“”], sessionId:””}
- **view state** render verify-challenge template
- **user interaction** event - enter challenge answer
- **user interaction** event - click answer
- **3rd party service** network - send {sessionId:””, answers:[]}
- **data** receive {status:”connected”, accounts:[{name:””,id:””,active:true},…]
- **view state** render account template
- **view state** finally in a state to test

<button onClick=“javascript: document.querySelectorAll(‘li’).map(i => { if (i.innerHTML.indexOf(“token”) === -1 || i.innerHTML.indexOf(“{status:”) === -1}) { i.style.transition = “opacity 2s”; i.style.opacity = 0.1; })”>
What is ACTUALLY needed to render?
</button>

</div>
<div class=“height: 100%; flex-basis: 50%”>
![](./reactables-accounts-connected.png)
</div>
</div>

<p class=“notes”>
What is needed for the immediate concern of rendering this UI?
</p>

--

# Simplify

If all that is needed is 170 bytes of data, why do we have to go through this massive, long brittle process to get there?  

Can we make this simpler and more immediate?  

```javascript
{
  token: “abcdef1234567890”,
  {banks:[{
		id:1, 
		name:”bank 1”,
    status:”connected”, 
		accounts:[{name:”account name”,id:”2”,active:true
}]}
```
--

# Simplify - Immediate Mode

- First popularized by Casey Muratori (@cmuratori) in 2005 for GUI development for video games.
- Absolutely 0 permutations.
- It's a simple data mutation. **Given X do Y.**
- ReactJS is the only web framework with Immediate Mode.
- ReactJS's takes Casey's immediate mode and makes it even clearer, simpler and performant.
- validate data in it’s simplest form

<p class=“notes”>
Immediate mode GUIs simplify the process by only be a simple transformation from JSON data to HTML/CSS representation of that data.
React’s addition to the paradigm was creating a super efficient process to reduce the amount of work per data mutation and minimize interactions with the browser DOM API to be responsive in the browser.
Immediate mode GUIs are a massive shift in UI architecture to build things we never could have imagined. And do it in a much simpler non-interwoven manner. Just like building construction moved away from brick and mortar to steel I beams opened up the door to building massive skyscrapers. Immediate mode is our I beam.
</p>

--

# Immediate Mode - Simple transforms

Once we’ve eliminated the permutation from our system we can be __immediate__. We can be simple.

**Todo: video of navigating to bank connect over via the URL.**

<p class=“notes>
Now that we’ve removed the complexities we’re able to do things unimaginable. We can go directly to any state in the app as long as we have the right data.
</p>

--

# Immediate Mode - Simple transforms explained

No complex or connected boilerplate needed. Add some mixin that exposes a global object to pass in a new state to the component and some predefined JSON. 

With Uglify’s dead code elimination no trace of the modification is exposed to the production environment.

![](reactables-code.png)
![](reactables-stub.png)
![](reactables-elimated.png)


<p class=“notes>
We have a simple snapshot mixin to connect the URL with a stubbed out JSON file for a given state. When it’s run through dead-code elimination on Uglify, it removes all traces of the stubs and snapshot code.
</p>

--

# Immediate Mode Foundation - Simple is BETTER

* because we’ve vastly simplified out UI process we can build bigger and better things on top that would be insanely complex previously
* we’ve already seen in place state changes.

—

# Immediate Mode Foundation - Real-time dev cycles

Hot swapping code works seamlessly with immediate mode. The view updates within a second. There is no browser refresh so the state of the app is unchanged. Also this works as well with change event actions.

![](reactables-hot-swap-with-selection.mov)

<p class=“notes”>
Notice that in the video we selection some text in the browser. When the update comes in that text is still selected. Because that HTML didn’t change at all it was unaffected, just like the rest of the page.
</p>

—

# Immediate Mode Foundation - State Stubs

We’ve already seen injecting stubs into the view to get the app in any state possible. Since we have the ability to go to any possible state we can do things like add inline translations.

**TODO: video of yolo bookmarklet**

<p class=“notes”>
With inline i18n translations, none of the code goes to prod. A small script just looks for each a data-attribute on an HTML tag and replaces the content with data from the en.json file.
</p>

—

# Immediate Mode Foundation - Resurrection

**Time to Interact** should be the key metric when it comes to page speed. The bar is to have content that a person can interact with in under 2 seconds.  

With no permutations, React can run a synchronous function to render HTML server-side (without a headless browser) and sent HTML to the browser. This means no JS is run on the client to display content.   

**TODO: show image of exported file sizes and webpack require code to do chunking**

—

# Immediate Mode Foundation - Time travel

Add in immutable data and we have a highly efficient way to record large amount of data changes.  

Infinite undo/redo is as simple as a single allocation of the change, connecting it to the immutable data structure and maintaining a pointer to each of the head changes.

**TODO: show Goya undo/redo**

<p class=“notes”>
Immutable data talks very little memory and little allocation space for multiple changes to the same data structure. Rewinding and replaying states are simple with no interleaving with other parts of the code.
</p>

—

# Immediate Mode Foundation - Teleportation

Immediate Mode and efficient versioning you can store server-side and revive a session on another device.  

* Users seamlessly switch between devices.
* Bug fixing can no persist the entire process of mutations to the server and the developer only need to rewind and step through the code to find the bug.


**TODO: find that one video or dev restoration of client state**

—

# Immediate Mode Foundation - Resilience (not rigidity)

* Because data is now separate from the user and the ui, it’s much easier to test.
* Unit tests are wonderful at data mutations.
* Unit tests break down is when they are conflating testing data mutation with the rendering processing. For instance testing for the presents of a class doesn’t mean your view is positioned properly or that it’s laid out properly or that the browser renders it properly on the screen. 

<p class=“notes”>
When working on a project for Nike, they had a complex business algorithm for distributing quantities. The business group wanted a full month prior to release to test that app because they had had so many issues on previous releases.  
We decided to isolate the data those transformation and put over 250 unit tests on possible input and output for 150 lines of code. After the first release we never had a problem again ever with 2 massive UI changes in the application.
</p>

—

# Immediate Mode Foundation - Telekinesis

With unit tests focused on data it allows the UI to be changed easier while the data mutations still function properly.  

Permutations and processing is clear by just looking at the data that is used when driving a view.  

<p class=“notes”>
When working on Nike the UX designer wanted to remove a 4 step wizard that was in modals to a single page with all the data element exposed. Because we had out data separate from the views, it took 2 hours to reflow the app with complete functionality. And only needed CSS work after that.
</p>

—

# 2015 UI developer manifesto

* Immediate Mode is a MASSIVE revolution!
* Reduce permutations in the view layer and the data layer.
* Move to ANY state in the app **IMMEDIATELY**!
* Real-time dev cycles with hot swapping code.
* No asking for repo steps, just rewind and step through the debug steps.
* Resilient apps are better than ridge apps. Unit test your data mutations more effectively without conflating view rendering.
* Less complications, less steps, less bugs and have more fun!

—

# Be Immediate. - Megatome

**[Megatome (bit.ly/megatome)](http://bit.ly/megatome)**
 
* Megatome will handle a ton of bootstrapping to help with getting  you in immediate mode with a simple build command.
* Contains:
 * React for performant Immediate Mode.
 * Webpack dev server for real-time dev cycle (hot swap code), proxies to external back-end server.
 * Webpack for super easy code splitting, transpilation (ES6/SASS/what ever), stats on dependency graph, asset optimization.
 * React Router for seamless integration in webpack, passing data between routes, animatable page transitions and dynamic micro-routes (think of localize routes for different page states, like models).
 * ImmutableJS for super efficient data memory management.
 * Simplified webpack config for magnification, dead code elimination, source maps, long-term asset caching (on prod), pre-gzip and static code compilations (turn async code to sync and run 99% of the client code in Node for server-side HTML generation).

<p class=“notes”>
React’s immediate mode opens up these possibilities but webpack takes advantage of it. Web pack allows for hot swapping code and  chunks up javascript for more efficient loading. 
</p>
—

# Be Immediate. - 0 to 60

**[Megatome (bit.ly/megatome)](http://bit.ly/megatome)**

```
brew install node
npm install -g yo
npm install -g generator-megatome
yo megatome My-App-Name
npm start
```

**TODO: video of installing and running Megatome**

—

# Permutations lead to complexity. Complexity leads to suffering. - @ImmediateYoda 

# Go. Be Immediate. - @puppybits

<span style=“font-size:10px”>We are hiring. Work with Immediate Mode, ReactJS, super fast page loads speed, hi-performant rendering responsiveness, highly composible react web components library, open source your work, iterate with great designers.</span>
—
















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
* 2,000 millisecond TTI (time to interact) decreased revenue by -4%. <sup>[bing, amazon, google][page-load-stats]</sup>
* 3,000 milliseconds will lose 40% of people. <sup>[yotta][bounce-rate]</sup>
* 100 milliseconds direct manipulation. interactions makes user feel the system is reacting properly (no special feedback needed). <sup>[Jakob Nielson PhD][ui-limits]</sup>
* 1,000 milliseconds interrupts the users flow of thought. <sup>[Jakob Nielson PhD][ui-limits]</sup>
* 10,000 milliseconds lose the user’s attention. <sup>[Jakob Nielson PhD][ui-limits]</sup>
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
### NOTES
* immediate mode benefits - hybrid server-client app for fast loads, state debugging, fast hot swapping, 
# Immediate Mode - Snapshots the UI 

![Data](images/verify-json.png)
![View](images/verify-view.png)

* This is most critical difference with React.
* Explain possible combinations of an action with possible permutations. Each permutation has a logarithmic increase is the number of possibilities and becomes increasingly hard.
* Most UIs are a permutation of all changes since the start of the system
* The complex order of operations make it’s more brittle to build things on top of this model.
* If you've ever had to do full regressions because different parts of you app cause bugs at other ends of your app, that's because of the permutation effect. 

——
### Immediate Mode - UI as a side effect.

![Data](images/verify-json-creds.png)
![View](images/verify-view-creds.png)

* React views are a product of the last global data snapshot against current data snapshot
* Complex permutations are able to be handled elsewhere. With UI, human interactions and time it makes it easier to isolate and unit test data transforms. 

**TODO: make example of CSS transform order of operations versus additive settings.**

——
### UI as a side effect - Hot swapping code

**TODO: add fnc to validate password on type. use hot swap to update view and test.**

**TODO: change fnc to validate on button press. use hot swap to update view and test.**


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
