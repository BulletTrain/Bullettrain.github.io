---
title: 设计模式之Memento Pattern(备忘录模式)
date: 2016-02-04 09:41:38
categories: 设计模式
tags: Memento Pattern
copyright: true
---

## 简介

在不破坏封装的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，这样就可以在以后将对象恢复至原先保存的状态。他是一种`对象行为型模式`。

## 适用场景

- 类似于撤销功能的实现，保存一个对象在某个时间的部分状态或全部状态，当以后需要它时就可以恢复至先前的状态。
- 对对象历史状态的封装、避免将对象的历史状态的实现细节暴露给外界。

## UML类图

![备忘录UML类图](https://www.flyada.com/images/%E5%A4%87%E5%BF%98%E5%BD%95UML%E7%B1%BB%E5%9B%BE.png)

## 参与者

- 备忘录模式角色之原发器类: Originator.java
  
  ``` java
  /**
   * 备忘录模式角色之  原发器<br/>
   * 含有内部需要保存的状态属性
   */
  public class Originator {
  	
  	private String state;
  
  	/**
  	 * 创建一个备忘录对象 
  	 * @return
  	 */
  	public Memento createMemento(){
  		return new Memento(this);
  	}
  	
  	/**
  	 * 根据备忘录对象恢复原发器先前状态  
  	 * @param o
  	 */
  	public void restoreMemento(Memento o){
  		this.state = o.getState();
  	}
  	
  	public String getState() {
  		return state;
  	}
  
  	public void setState(String state) {
  		this.state = state;
  	}
  	
  }
  ```


- 备忘录模式角色之备忘录类: Memento.java
  
  ``` java
  /**
   * 备忘录模式角色之  备忘录<br/>
   * 存储原发器的内部状态
   */
  public class Memento {
  
  	private String state;
  
  	public Memento(Originator originator) {
  		this.state = originator.getState();
  	}
  
  	public String getState() {
  		return state;
  	}
  
  	public void setState(String state) {
  		this.state = state;
  	}
  	
  }
  ```


- 备忘录模式角色之 备忘录管理者或负责人类：Caretaker.java

``` java
/**
 * 备忘录模式角色之 备忘录管理者或负责人<br/>
 * 只负责存储备忘录对象，而不能修改备忘录对象，也无须知道备忘录对象的实现细节<br/>
 * <b>扩展：<br/>
 * 如果要实现可以多步撤销的备忘录模式 则只需要在此类中使用一个数组集合来装载每一步状态的备忘录对象 如：<br/>
 *  // 定义一个数组集合来存储多个备忘录对象  <br/>
	List<Memento> mementos = new ArrayList<Memento>();
 * </b>
 */
public class Caretaker {

	private Memento memento;

	public Memento getMemento() {
		return memento;
	}

	public void setMemento(Memento memento) {
		this.memento = memento;
	}
	
}
```

- 客户端测试类Client.java
  
  ``` java
  public class Client {
  
  	public static void main(String[] args) {
  		Caretaker caretaker = new Caretaker();
  		Originator originator = new Originator();
  		// 初始化状态标识 "0"
  		originator.setState("0");
  		// 创建状态为"0"的备忘录对象
  		Memento memento_1 = originator.createMemento();
  		// 将记录了Originator状态的备忘录 交给 Caretaker备忘录管理者储存
  		caretaker.setMemento(memento_1);
  		showState(originator);
  		
  		System.out.println("----- 更改原发器的状态 -----");
  		// 更改原发器的状态标识为"1"
  		originator.setState("1");
  		showState(originator);
  		
  		System.out.println("----- 撤销至原发器的先前状态 -----");
  		originator.restoreMemento(caretaker.getMemento());
  		showState(originator);
  	}
  
  	
  	private static void showState(Originator originator) {
  		System.out.println("Originator 的当前状态：" + originator.getState());
  	}
  
  }
  ```


- 运行结果
  
  ``` java
  Originator 的当前状态：0
  ----- 更改原发器的状态 -----
  Originator 的当前状态：1
  ----- 撤销至原发器的先前状态 -----
  Originator 的当前状态：0
  ```