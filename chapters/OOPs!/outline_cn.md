OOPs! _ Object Orient Programming, OOP: a practical approach in OF
=========

OOPs! _ 面向对象编程, OOP: 在 OF 中的实践方法
=========

notes:
注意:
- trying to not be redundant with Josh's Chapter, this would be a direct approach / practical example to OOP in OF
- 试着在 Jhon的章节里别做多余的工作, 这会是一个 OF 中最直接的 OOP 方法 / 实践例子。
- this will be loosely based on and extended from the OOPs tutorial
- 这里将基本基于 OOPs 教程并以此做拓展。
http://www.openframeworks.cc/tutorials/first%20steps/003_ooops_object_oriented_programming.html

- brief notes about OOP:
- 关于 OOP 简短的介绍
	- what is OOP
	- 什么事 OOP
		- Objects and Classes 
		- 对象与类
		- example of basic class and object
		- 基础的类与对象的例子
			- make a very simple class with setup, update and draw methods (simple particle ?)
			- 用 setup, update, 和 draw 方法写一个简单的类 (简单的粒子 ?)
			- make your object from the class
			- 从类中创建你的对象
			- make multiple objects the hard way
			- 用笨方法创建复合对象
			- make an array of objects (the easy way) _ using a CONSTANT
			- 创建一个对象的数组 (用简单的方式) _ 用一个 CONSTANT
			- make an array of objects (the easy way) _ array using pointers
			- 创建一个对象的数组 (用简单的方式) _ 使用指针于数组
			- make an array of objects (the easy way) _ dynamic size of array using stl::vector
			- 创建一个对象的数组 (用简单的方式) _ 使用 stl::vector 于数组的动态范围
			- make an array of objects (the easy way) _ dynamic size of array using stl::vector and destroying objects if they leave the screen
			- 创建一个对象的数组 (用简单的方式) _ 使用 stl::vector 于数组的动态范围, 以及如何对象离开屏幕就消除它	
	- more OOPs! : Polymorphism
	- 更多的 OOPs! : 多形性
		- What is it / what does it mean ?
		- 多形性是什么 / 它又意味着什么 ?
		- example of basic class and object (polymorphism)
		- 基础类和对象的例子 (多形性)
			- make a simple class (simple particle?)
			- 创建一个简单的类 (简单的粒子 ?)
			- make different objects from the same class
			- 从同一个类中创建不同的对象