#HelloWorld.html

                     <!DOCTYPE html>
                     <html>
                     <head>
                     <meta charset="UTF-8"/>
                     <title>Hello React!</title>
                     <script src="build/react.js"></script>
                     <script src="build/react-dom.js"></script>
                     <script src="https://npmcdn.com/babel-core@5.8.38/browser.min.js"></script>
                     </head>
                     <body>
                     <div id="example"></div>
                     <script type="text/babel">
                       ReactDOM.render(
                        <h1>Hello, world!</h1>,
                        document.getElementById('example')
                        );
                        </script>
                      </body>
                    </html>
                    
The XML syntax inside of JavaScript is called JSX; check out the JSX syntax to learn more about it. In order to translate it
to vanilla JavaScript, we use <script type="text/babel"> and include Babel to actually perform the transformation in the browser.
Open the HTML from a browser and you should already be seeing the greeting!

#Separate File
Your React JSX code can live in a separate file. Create the following src/helloworld.js.

                   ReactDOM.render(
                   <h1>Hello, World!</h1>
                   document.getElementById('example');
                   );

Then reference it from the helloworld.html

                   <script type="text/babel" src="src/helloworld.js"></script>
                   
Note that some browsers will fail to load the file unless it's served via HTTP.

#Getting Started
Included in the server package discussed above is an HTML file which we'll work in. Open up public/index.html in your
favorite editor.

                     <!DOCTYPE HTML>
                     <html>
                     <head>
                     <meta charset="utf-8">
                     <title>React Tutorial</title>
                     <script src="https://npmcdn.com/react@15.3.0/dist/react.js"></script>
                     <script src="https://npmcdn.com/react-dom@15.3.0/dist/react-dom.js"></script>
                     <script src="https://npmcdn.com/babel-core@5.8.38/browser.min.js"></script>
                     <script src="https://npmcdn.com/jquery@3.1.0/dist/jquery.min.js"></script>
                     <script src="https://npmcdn.com/remarkable@1.6.2/dist/remarkable.min.js"></script>
                     </head>
                     <body>
                       <div id="content"></div>
                       <script type="text/babel" src="scripts/example.js"></script>
                       <script type="text/babel">
                       </script>
                     </body>
                     </html>
                     
#Your First Component
React is all about modular, resuable components. For our comment box example, we'll have the following component structure.

                    -CommentBox
                      -CommentList
                       -Comment
                    -CommentForm
                    
Let's build the CommentBox component, which is just a simple <div>:

                    tutorial1.js
                    var commentBox = React.createClass({
                     render: function() {
                      return (
                       <div className="commentBox">
                       Hello, World! I am a commentBox.
                       </div>
                       );
                      }
                    });
                    ReactDOM.render(
                     <CommentBox />
                     document.getElementById('contnet')
                     );
                     
Note that native HTML element names start with a lowercase letter, while custom React class names begin with
an uppercase letter.

#JSX Syntax
The first thing you'll notice is the XML-ish syntax in your JavaScript. We have a simple precompiler that translates
the syntatic sugar to the plain JavaScript

                    //tutorial1-raw.js
                     var commentBox=React.createClass({displayName: 'CommentBox',
                      render: function() {
                       return(
                        React.createElement('div', {className: 'commentBox'},
                          "Hello World! I am a CommentBox."
                          )
                        );
                      }
                    });
                    ReactDOM.render(
                     React.createElement(CommentBox, null),
                     document.getElementById('content')
                    );
                    
#What's Going On
We pass some methods in a JavaScript object to React.createClass() to create a new React component. The most important
of these methods is called a render which returns a tree of React components that will eventually render to HTML.

The <div> tags are not actual DOM nodes; they are instantiations of React <div> components. You can think of these as
markers or pieces of data that React knows to handle. React is safe. We are not generating HTML strings so XSS protection
is the default.

You do not have to return basic HTML. You can return a tree of components that you (or someone else) built. This is what
makes React composable: a key tenant of maintainable frontends. 

ReactDOM.render() instantiates the root component, starts the framework, and injects the markup into a raw DOM element,
provided as the second argument.

The ReactDOM module exposes DOM-specific methods, while React has the core tools shared by React on different platforms.

It is important that ReactDOM.render remain at the bottom of the script for this tutorial. ReactDOM.render should only be
called after the composite components have been defined.

#Composing Components
Let's build skeletons for CommentList and CommentForm which will, again, be simple <div>s. Add these two components to your
file, keeping the existing CommentBox declaration and ReactDOM.render call.

                         //tutorial2.js
                         var commentList = React.createClass({
                          render: function() {
                           return (
                            <div className="commentList">
                            Hello, World! I am a comment list.
                            </div>
                            );
                           }
                         });
                         var commentForm = React.createClass({
                          render: function() {
                           return(
                            <div className="commentForm">
                            Hello, World! I am a CommentForm.
                            </div>
                            );
                           }
                          });
                          
Next, update the commentBox to use these components:

                           //tutorial3.js
                           var CommentBox = React.createClass({
                            render: function() {
                             return (
                              <div className="commentBox">
                                <h1>Comments</h1>
                                <CommentList />
                                <CommentForm />
                                </div>
                                );
                              }
                            });
                            
Notice how we're mixing HTML tags and components we've built. HTML components are regular React components, just like the
ones you define, with one difference. The JSX compiler will automatically rewrite HTML tags to ReactDOM.createElement(tagName)
expressions and leave everything else alone. This is to prevent the pollution of the global namespace.

#Using Props
Let's create the Comment component, which will depend on data passed in from its parent. Data passed in from a parent
component is available as a 'property' on the child component. These properties are accessed through this.props. Using
props, we will be able to read the data passed to the Comment from CommentList, and render some markup:

                            //tutorial4.js
                            var Comment = React.createClass({
                             render: function() {
                              return (
                               <div className="comment">
                                <h2 className="commentAuthor">
                                 {this.props.Author}
                                </h2>
                                {this.props.children}
                                </div>
                                );
                              }
                            });
                            
By surrounding a JavaScript expression in braces inside JSX (as either an attribute or child), you can drop text or React
components into the tree. We access named attributes passed to the components as keys on this.props and any nested
elements as this.props.children.

#Comment Properties
Now that we have defined the Comment component, we will want to pass it to the author name and comment text. This allows us
to reuse the same code for each unique comment. Now let's add some comments with our CommentsList:

                                  //tutorial5.js
                                  var CommentList=React.createClass({
                                   render: function() {
                                    return (
                                    <div className="commentList">
                                     <Comment author="Pete Hunt">This is One Comment</Comment>
                                     <Comment author="Jordan Walke">This is *another* comment</Comment>
                                     </div>
                                      );
                                    }
                                  });
                                  
Note that we have passed some data from the parent CommentList component to the chil Comment components. For example,
we passed Pete Hunt (via an attribute) and This is one comment (via an XML-like child-node) to the first Comment. As noted
above, the Comment component will access these properties through this.props.author, and this.props.children.

#Adding Markdown

                                     

                           
                         
