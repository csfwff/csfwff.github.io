title: Kotlin大法好-->"_"下划线的妙用
date: '2019-03-21 18:54:47'
updated: '2021-01-05 16:39:53'
tags: [Kotlin, Android]
permalink: /articles/2019/03/21/1553165686993.html
cover: https://tmx.fishpi.cn/img/flowers-6213351_1920.jpg
categories: 
- 菜鸡修炼手册
---
![c8772df9de5b2637afabe3ae55f616c1.jpg](https://tmx.fishpi.cn/img/flowers-6213351_1920.jpg)

记录kotlin的学习，谨防忘记

---

_下划线平时用于在变量命名的时候作为单词的间隔
除此之外，在kotlin中还有别的用途

### 作为lambda的参数名称

```
view.setOnTouchListener{ v , event ->
	//do something
}
```

如果上述回调中，我们只用到了event而没有用到v的时候，就可以用_代替

```
view.setOnTouchListener{ _ , event ->
	//do something
}
```

当然，如果一个参数都没用到，那就自己省略参数部分就完事

### 作为解构声明参数

解构可以一次性建好多个变量，但是有时候会用不到其中一部分参数，那么解构的时候用_替代就可以了

```
data class Person(var name : String, var age : Int)
val person = Person("Tom", 20)
val (name, age) = person
```

这时候创建了name和age两个变量，如果不需要name，那么就可以用_代替name
`val (_, age)= person`

### 总结

总结一下就是，用不到但是必须有的时候，就用下划线代替

