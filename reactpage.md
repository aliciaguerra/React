#A Simple Component
React components implement a render() method that takes input data and returns what to display. The example uses an
XML-like syntax called JSX. Input data that is passed into the component can be accessed by render() via this.props().

JSX is optional and not required to use React. Try clicking on "Compiled JS" to see the raw JavaScript code produced
by the JSX compiler.

               var HelloMessage = React.createClass({
                render: function() {
                 return <div> Hello {this.props.name}</div>;
                 }
                });
                
                ReactDOM.render(<HelloMessage name="John" />, mountNode);
                
#A Stateful Component
In addition to taking input data (accessed via this.props), a component can maintain internal state data (accessed
via this.state). When a component's state data changes, the rendered markup will be updated by invoking render().

               var Timer = React.createClass({
                getInitialState: function() {
                return {secondsElapsed: 0};
                }
                tick: function() {
                 this.setState({secondsElapsed:this.state.secondsElapsed+1});
                },
                componentDidMount: function() {
                 this.interval=setInterval(this.tick, 10000);
                 },
                componentWillUnmount: function() {
                 clearInterval(this.interval);
                 },
                 render: function() {
                 return(
                 <div>Seconds Elapsed: {this.state.secondsElapsed}</div>
                    );
                   }
                  });
                  
                  ReactDOM.render(<Timer /> , mountNode);
                  
#An Application
Using props and state, we can put together a small todo application. This example uses state to track the current list of items
as well as the text the user has entered. Although event handlers appear to be handled inline, they will be collected and
implemented using event delegation.

                  var TodoList = React.createClass({
                   render: function(){
                    var createItem = function(item) {
                     return <li key={item.id}>{item.text}</li>
                     };
                     return <ul>{this.props.items.map(createItem}</ul>
                     }
                    });
                    
                  var TodoApp = React.createClass({
                   getInitialState: function() {
                    return {items:[], text: ''};
                    }
                  onChange: function(e) {
                    this.setState({text: e.target.value});
                  },
                  handleSubmit: function(e) {
                   e.preventDefault();
                   var nextItems = this.state.items.concat([text: this.state.text, id: Date.now()}]);
                   var nextText='';
                   this.setState({items: nextItems, text: nextText});
                   },
                   render: function() {
                    return (
                     <div>
                      <h3>TODO</h3>
                      <TodoList items={this.state.items} />
                      <form onSubmit{this.handleSubmit}>
                       <input onChange={this.onChange} value={this.state.text} />
                       <button>{'Add #' + (this.state.items.length + 1)}</button>
                       </form>
                       </div>
                       );
                      }
                    });
                  
                    ReactDOM.render(<TodoApp />, mountNode);
                    
#A Component Using External Plugins
React is flexible and provides hooks that allow you to interface with other libraries and frameworks. This example
uses remarkable, an external markdown library, to convert the textarea's value in realtime.

                   var MarkdownEditor = React.createClass({
                    getInitialState: function() {
                     return {value: 'Type some *markdown* here!'};
                     }
                    handleChange: function() {
                     this.setState({value: this.refs.textarea.value});
                    },
                    rawMarkup: function() {
                     var md = new Remarkable();
                     return {__html: md.render(this.state.value)};
                     },
                    render: function() {
                     return (
                      <div className="MarkdownEditor">
                      <h3>Input</h3>
                      <textarea
                       onChange={this.handleChange}
                       ref="textarea"
                       defaultValue={this.state.value} />
                      <h3>Output</h3>
                      <div
                       classname="content"
                       dangerouslySetInnerHTML={this.rawMarkup()}
                        />
                      </div>
                      );
                    }
                  });
                  
                  ReactDOM.render(<MarkdownEditor/>, mountNode);
                      
                     
                     
