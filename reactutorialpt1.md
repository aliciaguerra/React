JSX-allows us to write HTML-like syntax which gets transformed to lightweight JavaScript objects

Virtual DOM - A JavaScript representation of the actual DOM

React.createClass- the way in which you create a new component. 

render(method) - what we would like our HTML template to look like

ReactDOM.render - renders a React component to a DOM node

state - the internal data store (object) of a component

getInitialState - the way in which you set the initial state of the component

setState - A helper method for altering the state of the component

props - The data which is passed to the child component from the parent component

propTypes - allows you to control the presence, or types of certain props passed to the child components

getDefaultProps - Allows you to set default props for your component.

Component LifeCycle

componentWillMount - fired before the component will mount

componentDidMount - fired after the component mounted

componentWillRecieveProps - fired whenever there is a change to props

componentWillUnmount - fired before the component will unmount

#Creating your First Component (JSX, Virtual DOM, render, ReactDOM.render)
First, let's create our very first HelloWorld component so you can visualize how we use createClass to create a component.
createClass takes in an object. This object is what will specify the properties (render, getInitialState, propTypes) of the
component.

               var HelloWorld = React.createClass ({
                  render: function() {
                   return (
                    <div>
                    Hello World!
                    </div>
                    )
                  }
                });
                
                
                ReactDOM.render(<HelloWorld />, document.getElementById('app'));
                
Notice that the only method we're passing to the createClass is the render method. Every component is required to have a 
render method. The reason for that render is essentially the template for our component. So, in this example, the text will
show on the screen where this component is rendered Hello World! We've saved the result of calling our React.createClass
constructor into a variable called HelloWorld. We do this because later on we'll need to tell React to which element should
be rendered. This is when ReactDOM.render() comes into play. ReactDOM.render() takes in two arguments. The first argument is the
component you want to render, the second argument is the DOM node where you want to render the component (notice we're using 
ReactDOM.render() and not React.render(). This was a change in React .14 to make React more modular. It makes sense when you
think that React can render to more things than just a DOM element). In the above example, we're telling React to take our
HelloWorld component and render it to an element with an ID of an app. Because of the parent/child relations of React we
talked about earlier, you usually only have to use ReactDOM.render once in your application because by rendering the most
parent component, all child components will be rendered as well. If you want your whole app to be React, you would render
the component to document.body.

Now, at this point, you might feel a little wierd throwing around "HTML" into your JavaScript. Since you started learning web 
development, you were told to keep your logic out of the view, AKA keep your JavaScript uncoupled from your HTML. This paradigm
is strong, but it has some weaknesses. The "HTML" that you're writing in the render method isn't actually HTML, it's called JSX.
JSX simply allows us to write HTML syntax which gets transformed to lightweight JavaScript object. React is then able to take 
these JavaScript objects and from them form a "virtual DOM" or a JavaScript representation of the actual DOM. This creates a
win/win sitaution where you get accessiblity of templates with the power of JavaScript.

Looking at the example below, this is what the JSX is transformed to once React forms its transformation process.

Hello World Compoent in JS

                   var HelloWorld = React.createClass({
                    displayName: "HelloMessage",
                    render: function() {
                     return React.createElement("div", null, "Hello World");
                     }
                    });
                    
Now, you can forgo the JSX->JS transformation phase and write your React components like the code above, but as you can imagine,
that would be rather tricky. 

Up until this point, we haven't really emphasized the importance of this new virtual DOM paradigm that we're jumping into.
The reason we went with this React approach is because, since the virtual DOM is a JavaScript representation of the actual 
DOM, React can keep track of the differences between the current virtual DOM (computed after some changes) with the 
previous virtual DOM (computed before some data changes). React then isolates some changes between the new and old virtual 
DOM and then only updates 
