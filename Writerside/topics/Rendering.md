# Rendering

Rendering is the process of transforming the component code that you write into user interfaces.

**Choosing the right time and place to perform rendering is important for building a performant application**.

## Rendering in React
React has been and is the go-to library for building single page applications (SPAs).

In a single page application, when the client makes a request for a webpage, the server sends a single HTML page to the
browser, this HTML page usually contains a single div tag and a reference to a JavaScript file.

This JavaScript file contains everything your application needs to run, this includes the React library itself and your
application code.

This JavaScript file is downloaded when the browser finishes parsing the HTML file, the downloaded JavaScript code then
generates the HTML code on your computer and inserts it into the DOM under the root div element and you end up seeing
the user interface in the browser.

This method of rendering where the component code is transformed into a user interface directly within the browser
(the client) is known as **client-side rendering**.

### Client Side Rendering (CSR)
Client Side Rendering (CSR); is a rendering strategy where the component code is transformed into a user interface
directly within the browser.

#### Drawbacks of Client Side Rendering
- **SEO issues**: generating HTML that mainly contains a single div tag is not optimal for SEO, as it provides little
content for search engines to index.
- **Performance issues**: having the browser handle all the work, such as fetching data, computing the UI, and making
the HTML interactive, can slow things down. Users might sometimes see a blank page or a loading spinner while the page
is loading. This is particularly noticeable to users with a slow network connection.


To address the challenges of client side rendering, modern react frameworks like gatsby and next.js started looking into
server-side solutions.

## Server Side Rendering (SSR)
In Server Side Rendering, when a request comes in, instead of sending an empty HTML that depends on client side JS
to construct the page, the server takes charge of rendering the full HTML, this fully formed HTML is then sent directly
into the browser, since the HTML has already been generated on the server, the browser is able to quickly parse and
display it, improving initial page load time.

However, with this approach, the full interactivity of the page is on hold until the JavaScript bundle comprised of
React itself alongside your application specific code has been completely downloaded and executed by the browser. This
phase is known as **hydration**.

During hydration, React takes control in the browser, reconstructing the component tree in memory based on the static
HTML that was served. React carefully plans the placement of interactive elements within this tree. Then, React proceeds
to bind the necessary JS logic to these elements. This involves initializing the application state, attaching event 
handlers for actions such as clicks, and setting up any other dynamic functionalities required for a fully interactive
user experience.

### Server Side Rendering Solutions
Server side solutions can be categorized into two strategies:
- Static Site Generation (SSG).
- Server Side Rendering (SSR).

**Static Site Generation (SSG)** occurs at build time, when the application is deployed to the server. This results in
pages that are already rendered and ready to serve. It is ideal for content that doesn't change often, like blog posts.

**Server Side Rendering (SSR)** on the other hand, renders pages on-demand in response to user requests. It is suitable
for personalized content like social media feeds, where the HTML depends on the logged-in user.

### Drawbacks of SSR
- **You have to fetch everything before you can show anything**: components cannot start rendering or "wait" while data
is being loaded. If a component needs to fetch data from a database or another source, this fetching must be completed
before the server can begin rendering the page.
- **You have to hydrate everything before you can interact with anything**: react hydrates the component tree in a
single pass, meaning once it starts hydrating, it won't stop until it's finished with the entire tree. As a consequence,
all components must be hydrated before you can interact with any of them.

These issues create an "all or nothing" problem that spans from the server to the client, where each issue must be
resolved before moving to the next one.

This is inefficient if some parts of your app are slower than others, as is often the case in real world apps.

To address these issues, React 18 introduced Suspense SSR architecture.

## Suspense SSR Architecture
This architecture allows you to use the `<Suspense>` component to unlock two major SSR features:
- HTML streaming on the server
- Selective hydration on the client

### HTML streaming on the server
You don't have to fetch everything before you can show anything.

If a particular section delays the initial HTML, it can be seamlessly integrated into the stream later.

This is the essence of how suspense facilitates server-side HTML streaming.

### Selective hydration on the client
By wrapping the main section within `<Suspense>`, you've indicated to React that it should not prevent the rest of the
page form not just streaming but also from hydrating.

This feature, called **selective hydration** allows for the hydration of sections as they become available, before
the rest of the HTML and the JavaScript code are fully downloaded.


=========THIS PART (RENDERING) IS NOT COMPLETED YET============