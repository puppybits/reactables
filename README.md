# Simple, Immediate. 

—

# Simple, Immediate. 

* Simple - do one thing.
* **Im**-mediate - in
* Im-**mediate** - now

—

# Real world examples.

—

# Real World Task - edit turning accounts on/off
![](imgs/bank-states.png)

—

# Real World - Mocks
![](imgs/page-flow.png)

—

# Real World - How does a application work?

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

# Permutations

- **per**-mutation - for each
- per-**mutation** - mean change
- Each item and the __order__ of those items, trigger a different change
- very complex
- easy to break
- Easy for a bug at one end to affect another end

--

# Permutations - Why are they bad? 

--

# Permutations - Conflating concerns

- data
- backend persistence
- service-provider backend
- network loading
- view template rendering
- animations
- view states
- end user and UX designer

**Todo: show image of tasks and group into concerns**

--

# Simplify

### What can we distill down to the essence of what is needed?

--

# Simplify

**todo: show list of tasks and the page at the connected state**

* Circle the user token and bank data

--

# Simplify

If all that is needed is token and the bank, why do we have to go through this long brittle process to get there?

--

# Simplify - Immediate Mode

- First popularized by Casey Muratori (@cmuratori) in 2005 for GUI development for video games.
- Absolutely 0 permutations.
- It's a simple data mutation. **Given X do Y.**
- ReactJS is the only web framework with Immediate Mode
- ReactJS's takes Casey's immedate mode and makes it even clearer, simpler and performant.
- Just like building construction moved away from brick and mortar to steel i beams to build skyscrapers. Immediate mode is our I beam.

--

# Immediate Mode development

- What was needed for rendering the connected state? Token, bank, account  

**Todo: show json of data = image of page.**

- Is just a simple transform
- One you speak in JSON the other is the same thing in HTML/CSS

--

# Immediate Mode development

- How do we use Immediate Mode?
- Render(json) will give the exact same output every time. 

**TODO: show code of render cycle**

--

# Immediate Mode development - hot swapping

* No complex permutations, no setting variables, no walking through a ton of functions and processing instructions.
* Given data X create HTML Y.

**Todo: show blank page, pass in page, then render and JSON. Then page loads.**

--

# Immediate Mode development - Performance

* Great! So immediate mode can help me fix bugs faster.
* But that seems horribly CPU and memory intensive. The browser must be crazy slow.

--

# Immediate Mode development - Performance

* Great! So immediate mode can help me fix bugs faster.
* But that seems horribly CPU and memory intensive. The browser must be crazy slow.

**TODO: talk about the rendering pipeline that is triggers of data mutation.**

--

# Simple Foundation - Simple is BETTER

* because we’ve vastly simplified out UI process we can build bigger and better things on top that would be insanely complex previously
* we’ve already seen in place state changes.

--

# Simple Foundation - (near) Real-time dev cycles

* Because it’s just a simple function you can just replace the render function and have a new flow. 
* Hot swap webpack will replace your render function.

**Todo: show video of live updating CSS.**

* The page didn’t change so the state is the same. 
* This works with events as well. 
* It works with completely changing the HTML. 

**Todo: show video of live updating events.**

--

# Simple Foundation - State Stubs

* figure out all the states a component can get into
* when it’s a DEV/QA build add in a mix-in to expose the states
* with Uglify and dead code elimination no dev code will even go to prod
* Turn on snapshots and toggle through each state or pass an int and go directly to the state you want
* With an good i18n library you can even do inline translations and save out a JSON file

--

# Simple Foundation - Resurrection

* Since HTML is just a result of JSON we can do dynamically switch between rendering view templates on the server or the client.
* Why would I want to do that?
* super fast page load speeds!
* On the first load of a page, the server will render static HTML/CSS and send it to the client.
* This is super small and doesn’t take up any space so the page loads crazy fast.
* After the page is loaded React.js loads after the first render and attached **ON TOP OF** the server-side rendered HTML. 
* Transitioning to another page only requires a small XHR to download a 2K JS for the following page. And then we can even do animated transitions between pages.  

**TODO: show image of exported file sizes and webpack require code to do chunking**

--

# Simple Foundation - Time travel

* add in immutable data and we have a highly efficient way to record large amount of data changes
* infinite undo/redo is as simple as pushing/popping a json in an array  

**TODO: show Goya undo/redo**

--

# Simple Foundation - Teleportation

* Now that we can time travel without using lots of memory or CPU, what is we save that to the server?
* Revive any state and any point in the application flow.
* Production debugging and troubleshooting is as easily as loading a JSON with of every action the user ever took on your development machine.
* Rewind and step through every transformation of the data and UI to find the bug. No more repo step or saying it works on my machine. See the data and see the mutations.  

**TODO: find that damn video or dev restoration of client state**

--

# Simple Foundation - Resilience (not rigidity)

* because data is now separate from the user and the ui, it’s much easier to test.
* Unit tests are wonderful at seeing if the data changes from a to b after running function f.
* create unit tests on the inputs and output of data transformation
* no more conflating `is(ele.hasClass(“disable”)` with if the user really can’t click on it and if the user can’t go to the next step and does the data get in a bad state.

**TODO: tell story of Nike E-comm size runs. **

--

# Simple Foundation - Telekinesis

* The data transformation aren’t coupled to the view. 
* The data transforms are unit tests so we are secure that it can’t get into a bad state.
* UI is just a simple function of render(JSON).
* moving view elements and reflowing the process of the app is simple to do. 
* the developer can drop in at any state and progress from there with **NO prerequisites**.  

**TODO: tell story of Nike E-comm UX designer went from wizard with 3 models to single page and it took 2 hours to reflow a working app (minus the CSS).**

--

# 2015 UI developer manifesto

* Immediate Mode is a MASSIVE revolution!
* Unit test your data mutations and persistence without conflating UI, rendering responsiveness, network speeds or end-users/UX concerns!
* Take your dev cycle to (near) real-time!
* Move to ANY state in the app **IMMEDIATELY**!
* Stop giving repo steps. Just load the client’s history of mutations and step through the how the data/UI interacted.
* Production fixes should be simple to find and quick to fix. Each fix should be a data layer unit test so you’re app is increasing more stable.
* Work with designer to iterate on UIs quicker, with live data. 
* Less complications, less steps, less bugs and have more fun!

--

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

--

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

--

# Permutations lead to complexity. Complexity leads to suffering. - @ImmediateYoda 

# Go. Be Immediate. - @puppybits

--
