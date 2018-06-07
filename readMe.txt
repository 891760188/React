//为某个节点嵌入HTML
ReactDOM.render(
    <h1>Hello, world!</h1>,
    document.getElementById('example')
);
//在大括号中使用js表达式
ReactDOM.render(
    <div>
      <h1>{1+1}</h1>
      <h1>{i == 1 ? 'True!' : 'False'}</h1>
    </div>
    ,
    document.getElementById('example')
);

样式
React 推荐使用内联样式。我们可以使用 camelCase 语法来设置内联样式. React 会在指定元素数字后自动添加 px 。以下实例演示了为 h1 元素添加 myStyle 内联样式：
React 实例//注释需要写在花括号中
var myStyle = {
    fontSize: 100,
    color: '#FF0000'
};
ReactDOM.render(
 {/*注释...*/} 
    <h1 style = {myStyle}>菜鸟教程</h1>,
    document.getElementById('example')
);
数组
var arr = [
        <h1>菜鸟教程</h1>,
        <h2>学的不仅是技术，更是梦想！</h2>,
      ];
      ReactDOM.render(
        <div>{arr}</div>,
        document.getElementById('example')
      );
组件
var HelloMessage = React.createClass({
	  render: function() {
		return <h1>Hello World！{this.props.name}</h1>;
	  }
	});
	var Father = React.createClass({
		render : function(){
			return <HelloMessage name ={this.props.aaa}/>{/**接收属性值，调用组件*/}
		}
	});
ReactDOM.render(
	  <Father aaa="我是属性" />,{/**属性值的传递*/}
	  document.getElementById('example')
	);

React 里，只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）。
以下实例中创建了 LikeButton 组件，getInitialState 方法用于定义初始状态，也就是一个对象，这个对象可以通过 this.state 属性读取。当用户点击组件，导致状态变化，this.setState 方法就修改状态值，每次修改以后，自动调用 this.render 方法，再次渲染组件。
var LikeButton = React.createClass({
		getInitialState : function(){
			return {liked:false}
		},
		handleClick : function(){
			this.setState({liked:!this.state.liked},function(event){
				//该函数会在setState设置成功，且组件重新渲染后调用
			});
		},
		render:function(){
			var text = this.state.liked?'喜欢':'不喜欢' ;
			return (
				<p onClick={this.handleClick}>
						你{text}
				</p>
			);
		}
});
ReactDOM.render(<LikeButton/>,document.getElementById('example'));

React 组件 API
  ● 设置状态：setState  
  ● 替换状态：replaceState
  ● 设置属性：setProps
  ● 替换属性：replaceProps
  ● 强制更新：forceUpdate
  ● 获取DOM节点：findDOMNode
  ● 判断组件挂载状态：isMounted

生命周期的方法有：
  ● componentWillMount 在渲染前调用,在客户端也在服务端。
  ● componentDidMount : 在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。 如果你想和其他JavaScript框架一起使用，可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异部操作阻塞UI)。
  ● componentWillReceiveProps 在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化render时不会被调用。
  ● shouldComponentUpdate 返回一个布尔值。在组件接收到新的props或者state时被调用。在初始化时或者使用forceUpdate时不被调用。 
可以在你确认不需要更新组件时使用。
  ● componentWillUpdate在组件接收到新的props或者state但还没有render时被调用。在初始化时不会被调用。
  ● componentDidUpdate 在组件完成更新后立即调用。在初始化时不会被调用。
  ● componentWillUnmount在组件从 DOM 中移除的时候立刻被调用。

Ajax：
var UserGist = React.createClass({
	getInitialState: function() {
		debugger
	  return {
		username: '',
		lastGistUrl: ''
	  };
	},
	componentDidMount: function() {
		debugger
	  this.serverRequest = $.get(this.props.source, function (result) {
		var lastGist = result[0];
		this.setState({
		  username: lastGist.owner.login,
		  lastGistUrl: lastGist.html_url
		});
	  }.bind(this));
	},
	componentWillUnmount: function() {
	  this.serverRequest.abort();
	},
	render: function() {
	  return (
		<div>
		  {this.state.username} 用户最新的 Gist 共享地址：
		  <a href={this.state.lastGistUrl}>{this.state.lastGistUrl}</a>
		</div>
	  );
	}
  });
  ReactDOM.render(
	<UserGist source="https://api.github.com/users/octocat/gists" />,
	document.getElementById('example')
  );