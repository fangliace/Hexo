title: iOS中自定义UITableView的cell
date: 2015-11-17 17:27:44
categories: iOS
tags: [OC, tableViewCell]
---

> 当一个tableView里面的cell的高度不一样时，需要用代码来自定义里面的cell

<!--more-->

### 自定义cell的步骤 :###

1. 新建一个继承自UITableViewCell的类

2. 重写initWithStyle: reuseIdentifier: 方法
	
	* 添加所有需要显示的子控件(不需要设置子控件的数据和frame, 子控件应该添加到contentView中)
	* 进行子控件的属性设置(有些属性只需要设置一次, 比如字体和固定的图片)

3. 提供两个模型

	* 数据模型: 存放数据(例如name, icon等)
	* frame模型: 存放数据模型、所有子控件的frame(例如nameFrame, iconFrame)、cell的高度

4. 自定义cell拥有一个frame模型(不需要直接拥有数据模型)

5. 重写frame模型属性的setter方法, 在这个方法中设置子控件的显示数据和frame

6. frame模型数据的初始化已经采取懒加载的⽅方式(每一个cell对应的frame模型数据只加载⼀次)



