---
layout: post
title: "关于Ruby的self"
category: rails
tags: []
---
{% include JB/setup %}

Ruby的self在使用时需要多加小心，在不同的地方使用，效果是不一样的。

---

>Top level context

	1.9.3p362 :001 > p self
	main
 	=> main
    
这里，在irb里直接打出self，得到默认对象main。

---

>在类中

	1.9.3p362 :001 > class Person 
	1.9.3p362 :002?>   p self
	1.9.3p362 :003?> end
	Person
	=> Person
    
---

>在‘类方法’中

	1.9.3p362 :001 > class Person
	1.9.3p362 :002?>   def self.method
	1.9.3p362 :003?>     p self
	1.9.3p362 :004?>   end
	1.9.3p362 :005?> end
	 => nil 
	1.9.3p362 :006 > Person.method
	Person
	 => Person
此时得到的是Person这个Class。需要注意的是，这里self.method不需Person实例化，直接调用。

---

>在类的实例方法中

	1.9.3p362 :001 > class Person
	1.9.3p362 :002?>   def method
	1.9.3p362 :003?>     p self
	1.9.3p362 :004?>   end
	1.9.3p362 :005?> end
	 => nil 
	1.9.3p362 :006 > Person.new.method
	#<Person:0x00000001a19658>
	 => #<Person:0x00000001a19658>
此时得到的是实例化后的Peroson。
