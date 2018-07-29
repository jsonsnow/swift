#### swift协议
协议是描述某些属性和方法的接口。任何符合协议的类型，都应该使用合适的值田畴协议中定义的特定属性，并且实现其必要的方法。

```
protocol Queue {
	var count:Int {}
	mutating func push(_ element:Int)
	mutating fun pop() -> Int

}

```

此Queue协议描述了一个队列，它包含了整形的元素
在协议块里面，当描述某个属性时，我们必须指定改属性只是只读 {get} 还是既可读有可以写{get set}.这里，变量Count 是只读。

```
struct Container: Queue {

	private var items:[Int] = []
	var count: Int {
		return items.count
	}
	mutation func push(_ element: Int) {
	 	items.append(element)
	}
	mutaing func pop() -> Int {
		return items.removeFirst()
	}
}
```

仅有处理Int的容器才能符合此协议

通过使用关联类型特性来去掉这个限制，关联类型的工作方式类似泛型。

```
protocol Queue {

	associatedtype ItemType
	var count:Int {get}
	func push(_ element:ItemType)
	func pop() -> ItemType
	
}
```

```
class Container<Item>:Queue {

	private var items:[Item] = []
	var count:Int {
		return items.count
	}
	
	 func push(_ element: Item) {
        items.append(element)
    }

    func pop() -> Item {
        return items.removeFirst()
    }
}

```

#### 协议扩展
协议扩展个人认为是协议另一处精髓所在，扩展协议可以说提供了可选实现，并且还能进行条件指定

* 1、提供协议方法的默认实现和协议属性的默认值，从而使得他们_可选_。符合协议的类型可以提供他们自己的实现或者使用默认提供的
* 2、添加不在协议描述里的额外方法并且使用这些额外方法_"装饰"_任意符合此协议的类型。这特特效相当于多重继承了，但却比多重继承强


```
protocol ErrorHandler {
	func handler(error:Error)
}
```

```
 	struct Handler: ErrorHandler {
 		func handle(error: Error) {
 			print(error. localizedDescription)
 		}
 	}
```

通过扩展协议我们可以让这个实现成为默认的实现

```
extension ErrorHandler {

	func handle(error: Error) {
        print(error.localizedDescription)
    }
}
```

通过提供一个默认的实现，还可以使得handler方法变成可选

#### 条件扩展
Swift 可以使用where关键字添加这样的条件到协议里

```
extension ErrorHandler where Self: UIViewController {  
    func handle(error: Error) {
        let alert = UIAlertController(title: nil, message: error.localizedDescription, preferredStyle: .alert)
        let action = UIAlertAction(title: "OK", style: .cancel, handler: nil)
        alert.addAction(action)
        present(alert, animated: true, completion: nil)
    }
}
```
现在、任何符合ErrorHandler协议的试图控制器都有了handle方法的默认实现


__以上是我在学习swift协议内容觉得重要和精髓的部分__。


___



##### 什么是abi，这里以被大家污垢的swift abi为例进行讨论

##### 什么事ABI
ABI(Application Binray interfae):应用程序二进制接口 描述了应用程序和操作系统之间，一个程序和它的库之间，或者应用组成部分之间的底接口。

ABI:涵盖了各个细节,如：

* 数据类型的大小、布局和对齐
* 调用约定（控制着函数的参数如何传送以及如何接收返回值），列入，是所有的参数都通过栈传递，还是部分参数通过寄存器，那个寄存器用于那个函数参数；通过栈传递的第一个函数参数是先push到栈上还是最后
* 系统调用的编码和一个应用如何向操作系统进行系统调用；
* 以及在一个完整的操作系统ABI中，目标文件的二级制格式，程序库等。

它描述了应用程序与OS之间的底层接口，ABI涉及程序的各个方面。比如：目标文件格式，数据类型，数据对齐。以及调用约定，参数传递，返回值，系统调用号，如何实现系统调用。





