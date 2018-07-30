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

#####  一直被污垢的ABI不稳定探究

