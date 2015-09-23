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

