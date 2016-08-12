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
DOM and then only updates the real DOM with the necessary changes. In more layman's terms, because manipulating the actual
DOM is slow, React is able to maximize manipulations to the actual DOM by keeping track of a virtual DOM and only updating the real DOM when necessary and with only the necessary changes. Typically UI's have lots of state which makes managing state difficult. By re-rendering the virtual DOM every time any state change occurs, React makes it easier to think about what
state your application is in.

The process looks something like this,
Signal to notfiy our app some data has changed -> Re-render virtual DOM -> Diff previous virtual DOM with new virtual DOM
-> Only update real DOM with necessary changes

Because there's the transformation process from JSX to JS, you need to set up some sort of transformation phase you're
developing. In part 2 of this series, I'll introduce using Gulp with React and how to have your Gulp process include this
JSX to JS transformation and more. 

JSX- allows us to write HTML like syntax which gets transformed to lightweight JavaScript objects.

Virtual DOM - a JavaScript representation of the actual DOM

React.createClass - the way in which you create a new component

state - the internal data store (object) of component

getInitialState - the way in which we set the initial state of a component

setState -  a helper method for altering the state of a component

props - the data which is passed to the child component from the parent component

propTypes - Allows you to control the presence, or types of certain props passed to the child component

getDefaultProps - Allows you to set the default props for your component

Component Lifecycle

componentWillMount - fired before the component will mount

componentDidMount - fired after the component mounted

componentWillRecieveProps - fired whenever there is a change to props

componentWillUnmount - fired before the component will unmount

#Adding State to your Component (State)
Next on the list is "state". Earlier we talked about how managing user interfaces is difficult because they typically
have lots of different states. This area is where React really starts to shine. Each component has the ability to manage
its own state and pass its state down to child components if needed. Going back to the Twitter example from earlier, the
UserInfo component is responsible for managing the state (or data) of the users information. If another component also
needed this state/data but that state wasn't a direct child of the UserInfo component, then you would create another 
component that would be the direct parent of the UserInfo and the other component (or both components which required that
state), then you would pass the state down as props to the child components. In other words, if you have a multi component
hierarchy, a common parent component should manage the state and pass it down to its children components via props.

#Hello {name} with State

                   var HelloUser = React.createClass({
                    getInitialState: function() {
                     react {
                      userName: '@stylermcginnis33'
                      }
                    },
                   render: function() {
                    return (
                     <div>
                     Hello {this.userName}
                     </div>
                     )
                    }
                  });
                  
We've introduced some syntax with this example. The first one you'll notice is getInitialState method. From the definition
above, the getInitialState method is "the way in which you set the state of a component". In other words, getInitialState
returns an object which contains the state or data for our component. In the code above we're telling our component that
we want it to keep track of a username. This username can now be used isnide our component by {this.state.userName}, which
is exactly what we do in the render method. 

The last thing we talk about with state is that our component needs the ability to modify its own internal state.
We do this with a method called setState. Remember earlier when we talked about the re-rendering of virtual DOM whenever
the data changes?

Signal to notfiy our app some data has changed -> re-render virtual DOM -> diff previous virtual DOM and new virtual DOM
-> only update real DOM with necessary changes

That "signal to notify our app some data has changed" is actually just setState. Whenever setState is called, the virtual
DOM re-renders, the diff algorithm runs, and the real DOM is updated with the necessary changes.

As a sidenote, when we introduce setState in the code below, we're also going to introduce a few events in the list. Two
birds, one stone. 

So, in the next code sample, we're going to now have an input box that whenever someone types into it, it will automatically
update our state and change the userName.

#Hello {name} with Ability to Change State

                  var HelloUser = React.CreateClass({
                   getInitialState: function() {
                    return {
                    userName: '@tylermcginnis33'
                    }
                  };
                  handleChange: function(e) {
                   this.setState({
                   username: e.target.value
                   });
                   },
                  render: function() {
                   return {
                    <div>
                     Hello {this.state.userName} <br />
                     Change Name: <input type="text" value={this.state.username}
                     onChange={this.handleChange} />
                    </div>
                    )
                   }
                  });
                  
Now, we've introduced a few more things. The first thing is the handleChange method. The method is going to get called
every time a user types in the input box. When handleChanged is called, it's going to call setState to re-define our
username with whatever is typed into the input box (e.target.value). Remmeber, whenever setState is called, React creates
a new virtual DOM, does the diff, then updates the real DOM.

Now let's look at our render method. We've added a new line that contains an input field. The type of the input field is
obviously going to be "text". The value is going to be the value of our username which was originally defined in our
getInitialState method and will be updated in the handleChange method. Notice there is a new attribute you've probably 
never seen before, onChange. onChange is a React thing and it will call whatever method you specify every time the value in
the input box changes, in this case the method we specified was handleChange. 

The process for the code above would go soemthing like this.

A user goes types into the input box -> handleChange is invoked -> the state of our component is set to a new value
-> React re-renders the virtual DOM -> React Diffs the change -> Real DOM is updated

#Recieve State from Parent Component (props, propTypes, getDefaultProps)
We've talked about props a few times already since it's hard to really do much without them. By our definition above,
props is the data which is passed to the child component from the parent component. This allows for our React architecture
to stay pretty straight forward. Handle state in the highest most parent component which needs to use the specific data, 
and if you have a child component that also needs that data, pass it down as props. 

#Very Basic Props Example

          var HelloUser = React.createClass({
            render: function() {
             return (
             <div>Hello, {this.props.name}</div>
             )
            }
          });
          
          ReactDOM.render(<HelloUser name="Tylor" />),
          document.getElementById('app'));
          
#Passing a list to Child Component

<h1>The Parent Component</h1>

          var FriendsContainer = React.createClass({
           getInitialState: function() {
           return {
            name: 'Tyler McGinnis',
            friends: ['Jake Lingwall', 'Murphy Randall', 'Merrick
            Christensen']
            }
          };
          render: function() {
           return (
            <div>
             <h3>Name: {this.state.name}</h3>
             <ShowList names={this.state.names} />
            </div>
            )
           }
          });

There really isn't going on in this component that we haven't seen before. We have an initial state and we pass part of that
intial state to another component. The majority of this new code will come from this child component.

<h1>The Child Component</h1>

          var showList = React.createClass({
           render: function() {
            var listItems = this.props.names.map(function(friends){
             return <li> {friend} </li>;
            });
          return (
            <div>
            <h3>Friends</h3>
            <ul>
            {listItems}
            </ul>
            </div>
            )
            }
            });
            
Remember that the code tht gets returned from our render method is a real representation of what the DOM could look like.
If you're not familiar with Array.prototype.map, this code might look a little wonky. All map does is create a new array,
calls our callback function on each item in the array, and fills the new array with the result of the calling the callback
function on each function.


#Array.prototype.map
        
          var friends = ['Jake Lingwall', 'Murphy Randall', 'Merrick Christensen'];
          var listItems = friends.map(function(friend) {
           return "<li> " + friend + "</li>";
           });
          console.log(listItems); //["<li> Jake Lingwall</li>", "<li>Murphy Randall</li>", "<li>Merrick Christensen</li>"];
          
The console.log above returns ["<li>Jake Lingwall</li>", "<li>Murphy Randall</li>", "<li>Merrick Christensen</li>"].
Notice that all that happened was we made a new array and added <li></li> to each item in the original array.

What's great about map is that it fits perfectly into React (and it's built into JavaScript). So in our child component
again, we're mapping over names, wrapping each in a pair of <li> tags, and saving that to the listItems variable. Then,
our render method returns an unordered list with all our friends.

Let's look at one example before we stop talking about props. It's important to understand that wherever the data lives, is
the exact place you want to manipulate data. This keeps it simple to reason about your data. All getter/setter methods for
a certain piece of data will always be in the same component where the data was defined. If you needed to manipulate some
piece of data outside where the data lives, you'd pass the getter/setter method into that component as props.

#Passing a List to a Child Component with a Setter Method

           var FriendsContainer = React.createClass({
            getInitialState: function() {
             return {
              name: 'Tyler McGinnis',
              friends: ['Jake Lingwall', 'Murphy Randall', 'Merrick Christensen'];
              }
            };
            
          addFriend: function(friend){
           this.setState({
            friends: this.state.friends.concat([friend])
            });
          },
          
          render: function(){
           return(
              <div>
           <h3> Name: {this.state.name}</h3>
           <AddFriend addNew={this.addFriend}>
           <ShowList names={this.state.friend}/>
               </div>
               )
             }
           });
           
           var addFriend = React.createClass({
            getInitialState: function(){
            return {
             newFriend: ''
             }
           };
           
           updateNewFriend: function(e){
            setState({
             newFriend: e.target.value
             });
            };
            
            handleAddNew: function(){
             this.props.addNew(this.state.newFriend);
             this.setState({
              new Friend: ''
             });
             };
             
             render: function() {
              return (
              <div>
              <input type="text" value={this.state.newFriend} onChange={this.updateNewFriend} /}
              <button onClick={this.addHandleNew}>Add Friend</button>
              </div>
                    );
                  }  
                });
                
              var showList = React.createClass({
               render: function(){
                var listItems = this.prop.names.map(function(friend) {
                 return <li> {friend} </li>;
                });
                return (
                <div>
                <h3>Friends</h3>
                <ul>
                <listItems>
                </ul>
                </div>
                  )
                 }
                });
 
 You'll notice that the code above is mostly the same as the previous example, except now we have the ability to add a 
 friend's name to the list. Notice how I created a new addFriend component that manages the new friend we're going to add.
 The reason for this is because the parent component (FriendContainer) doesn't care about the new friend you're adding,
 it only cares about all of your friends as a whole (the friends array). However, because we're sticking with the rule of only manipulate your data from the component that cares about it, we've passed the addFriend method down into our addFriend
 component as a prop and we call it with the newFriend once the handleAddNew method is called.
 
 Before we move on from props, I want to cover two more React features regarding props. There are propTypes andgetDefaultTypes. I won't go into much detail here because both are pretty straight forward.
 
 propTypes allow you to control the presence , or types of certain props passed to the child components. With propTypes,
 you can specify that certain props are required or that certain props be a specific type.
 
 getDefaultProps allow you to specify a deafult (or backup) value for certain props just in case those props are 
 never passed into the component.
 
 I've modified our example from earlier to now, using propTypes, require that addFriend is a function and that it's passed
 into the addFriend component. I've also, using getDefaultProps, specified that if no array of friends is given to the
 ShowList component, it will default to an empty array.
 
 #Passing a List to a Child Component with a Setter Method with Default Props and Type Checking
 
                 var FriendsContainer = React.CreateClass({
                     getInitialState: function() {
                      return {
                        name: 'Tyler McGinnis',
                        friends: ['Jake Lingwall', 'Murphy Randall', 'Merrick
                                   Christensen'];
                                  }
                                };
                      addFriend: function(friend) {
                         this.setState({
                           friends: this.state.friends.concat([friend])
                           });
                          },
                          
                      render: function(){
                        return (
                         <div>
                          <h3>Name: {this.state.name}</h3>
                          <AddFriend addNew={this.addFriend} />
                          <ShowList names={this.state.friends} />
                         </div>
                          )
                         }
                        });
                        
                        var addFriend=React.createClass({
                         getInitialState: function(){
                          return {
                           newFriend: ''
                           }
                          },
                        propTypes: {
                         addNew: React.PropTypes.func.isRequired
                         },
                        updateNewFriend: function(e) {
                         this.State({
                          newFriend: e.target.value
                          });
                        },
                        handleAddNew: function() {
                         this.props.addNew(this.state.newFriend);
                         this.setState({
                          newFriend: ''
                          });
                        },
                        render:function() {
                         return(
                         <div>
                         <input type="text" value={this.state.newFriend} onChange=
                          {this.UpdateNewFriend} />
                         <button onClick={this.handleAddNew}>Add Friend</button>
                         </div>
                           );
                          }
                        });
                        
                        var showList=React.createClass({
                         getDefaultProps: function() {
                          return {
                           names: []
                           }
                          },
                          
                        render: function() {
                         var listItems = this.props.names.map(function(friend){
                          return <li> {friend} </li>;
                          });
                         return(
                          <div>
                           <h3>Friends</h3>
                           <ul>
                           {listItems}
                           </ul>
                           </div>
                           )
                          }
                        });
                        
#Component Lifecycle
Each component you make will have its own lifecycle events that are useful for various things. For example, if we wanted
to make an ajax request on the initial render and fetch some data, where would we do that? Or, if they wanted to run
some logic whenever the props changed, how do we do that? The different lifecycle events are the answers to both of these.

               var FriendsContainer=React.createClass({
                getInitialState: function(){
                 alert('In getInitialState'),
                 return {
                  name: 'Tyler McGinnis'
                  }
                },
                
               //Invoke once before first render
               componentWillMount: function(){
               //Calling setState here does not cause a re-render
               alert('In Component Will Mount');
               },
               
               //Invoked once after the first render
               componentDidMount: function() {
                //You now have access to this.getDOMNode
                alert('In Component Did Mount');
                },
                
              //Invoked whenever there is a prop change
               //Called BEFORE Render
               componentWillRecieveProps: function(nextProps){
                //Not called for the initial render
                //Previous props can be accessed by this.props
                //Calling setState here does not trigger an additional re-render
                alert('In component will recieve props');
                },
                
              //Called IMMEDIATELY before a component is unmounted 
                componentWillUnmount: function(){},
                
                render: function(){
                 return (
                  <div>
                   Hello, {this.state.name}
                  </div>
                  )
                }
              });
              
  componentWillMount: Invoked once before the initial render. If you were to call setState here, no re-render would be
  invoked. An example of this would be if you're using a service like firebase, you'd set up a reference to your firebase
  database here since it's only invoked once on the initial reader.
  
  componentDidMount: Invoked once after the single reader. Because the component has already been invoked when this method
  is invoked, you have access to the virtual DOM if you need it. You do that by calling this.getDOMNode(). Now it might 
  seem like if you wanted to make AJAX requests you would do that in componentWillMount, but the devs at facebook actually
  recommend you do that in componentDidMount. So this is the lifecycle event where you'll be making your AJAX requests
  to fetch some data.
  
  componentWillRecieveProps: This life cycle is not called on the intial render, but is instead called whenever there is
  a change to props. Use this method as a way to react to a prop change before render() is called by updating the state
  with setState.
  
  componentWillUnmount-This life cycle is invoked immediately before a component is unmounted from the DOM. This is where 
  you can do necessary cleanup. For example, going back to our firebase example this is the lifecycle event where you would
  clean up your firebase reference you set in componentWillMount.
          
          
            
