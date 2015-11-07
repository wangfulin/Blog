# 设计模式：发布/订阅模式解析

> **原文：**[Design Patterns:PubSub Explained](http://abdulapopoola.com/2013/03/12/design-patterns-pub-sub-explained/)


## 介绍

这个模式用来作为中间人，一个把发布者和订阅者架接在一起的代理。发布者是当完成某些过程的时候触发事件的对象，订阅者是希望当发布者发布的时候希望被通知的对象。

生活中有一个很好地例子，广播电台，人们会把频道调到他们最喜欢的节目。广播站不知道观众听得是什么或者他们正在听什么。他只需要发布他们的节目就可以啦。观众也不知道广播站制作节目的过程。他们只要在他们最喜欢的节目运行的时候把台调到对应的频道或者告知朋友就行。

发布/订阅者模式实现了松耦合：你可以让发布者发布消息，订阅者接受消息而不是寻找一种方式把两个分离的系统连接在一起。

## 优势

- 松耦合

发布者不需要知道订阅者的数量，订阅者听得话题或者订阅者是通过什么方式运行的。他们能够相互独立地运行，这样就可以让你分开开发这两部分而不需要担心对状态或实现的任何细微的影响。

- 可扩展性

发布/订阅模式可以让系统在无论什么时候无法负载的时候扩展

- 更干净地设计

充分地利用好发布/订阅模式，你不得不深入地思考不同的组件是如何交互的。这通常会让我们有更干净地设计因为我们对解耦和松耦合的强调。

- 灵活性

你不需要担心不同的组件是如何组合在一起的。只要他们共同遵守一份协议

- 容易测试

你可以很好地找出发布者或订阅者是否会得到错误的信息

## 缺点

发布/订阅模式最大的有点是解耦，但同时也是最大的缺点：
	
- 中间人也许不会通知系统消息传送的状态。所以我们无法知道消息传送是成功的还是失败的。紧耦合是需要保证这一点的。

- 发布者不知道订阅者的状态，反之亦然，这样的话，你根本不知道在另一端是否会没有问题？

- 随着订阅者和发布者数量的增加，不断增加的消息传送回导致架构的不稳定，容易在负载大的时候出问题

- 攻击者（恶意的发布者）能够入侵系统并且撕开它。这会导致恶意的消息被发布，订阅者能够获得他们以前并不能获得的消息。

- 更新发布者和订阅者的关系会是一个很难的问题，因为毕竟他们根本不认识对方。

- 需要中间人/代理商，消息规范和相关的规则会给系统增加一些复杂度

## 结论

现实没有银弹，但是这个模式是设计松耦合系统的很好地方式。这和RSS,Atom和PubSubHubbub的思想一样。

发布/订阅模式例子（Javascript）

	var makePubSub=function(){
		var callbacks={},
		publish=function(){
			//Turn arguments object into real array
			var args=Array.prototype.slice.call(arguments,0);

			//Extract the event name which is the first entry
			var ev=args.shift();

			//Return if callbacks object doesn't contain
			//any entry for event
			var list,i,l;
			if(!callbacks[ev]){
				return this;
			}
			list=callbacks[ev];

			//Invoke the callbacks,passing in the rest of parameters
			for(i=0,l=list.length;i<l;i++){
				list[i].apply(this,args);
			}

			return this;
		},
		subscribe=function(ev,callback){
			//Check if ev is already registered
			//If it isn't create an array entry for it
			if(!callbacks[ev]){
				callbacks[ev]=[];
			}
			callbacks[ev].push(callback);
			return this;
		};

		return {pub:publish,sub:subscribe};
	}

	test=makePubSub();
	test.sub("alert",function(){alert("hell0");})
	test.pub("alert");
