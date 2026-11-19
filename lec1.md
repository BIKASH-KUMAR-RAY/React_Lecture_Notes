# What is react?
->It is a javascript library.

React is nothing bas ek js ka object hi hai

# React's job->The "What" -Its job is only to describe what the UI should look like.It doestn create DOM elements .it creates light weight Javascript objects that act as blueprints.

## React DOm's job:The "How" -this is the renderer.Its job is to take blueprint from React and actually build the UI for a specific platform

React hamare DOM manipulation se kuch bhi related kaam nahi karta react dom karta hai cuz react aur bhi cheezon mai use hota hai mobile app development mai pdf maker mai so waha bhi woh bas ui banane ke kaam, aata hai

React.createElement(tag,properties,children);

```jsx
const element = React.createElement('h1', { id: 'title' }, 'Hello Coder Army');

const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(element);


```