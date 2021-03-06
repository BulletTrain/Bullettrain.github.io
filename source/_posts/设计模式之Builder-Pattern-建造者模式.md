title: 设计模式之Builder Pattern(建造者模式)
date: 2016-02-03 16:37:27
categories: 设计模式
tags: Builder Pattern
copyright: true
---

## 简介

将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。核心在于如何一步步构建一个包含多个组成部件的完整对象，使用相同的构建过程构建不同的产品，建造者模式是一种对象创建型模式。

## 适用场景

复杂对象的组装和创建

- 用于复杂对象的组装和创建、对象间通常具有较多的共同点或是组成部分相似
- 需要生成的对象的属性相互依赖，需要指定其生成顺序的情况
- 对象的创建过程独立于创建该对象的类，隔离复杂的对象的创建和使用，并使得相同的创建过程可以创建不同的产品

## UML类图结构

![UML类图](https://www.flyada.com/images/Build%20Parttern%20UML.png)

## 参与者

- Builder(抽象建造者): 它为创建一个产品Product对象的各个部件指定抽象接口,并有方法返回产品对象
- ConcreteBuilder(具体建造者): 它实现了Builder接口或抽象方法，实现各个部件的具体构造和装配方法，定义并明确它所创建的复杂对象
- Product(产品角色):  它是被构建的复杂对象，通常包含多个组成部件
- Director(指挥者): 它负责安排复杂对象的构造次序，指挥者与抽象构造者之间存在关联关系，可以在其construct()建造方法中调用建造者对象的部件构造与装配方法，完成复杂对象的建造

## 用例学习

- 被建造的对象: 产品类 Product.java
  
  ``` java
  /**
   * 被建造的对象：产品
   */
  public class Product {
  	// 产品部件A
  	private String partA;
  	// 产品部件B
      private String partB;
      // 产品部件C
   	private String partC;
   	
  	public String getPartA() {
  		return partA;
  	}
  	
  	public void setPartA(String partA) {
  		this.partA = partA;
  	}
  	
  	public String getPartB() {
  		return partB;
  	}
  	
  	public void setPartB(String partB) {
  		this.partB = partB;
  	}
  	
  	public String getPartC() {
  		return partC;
  	}
  	
  	public void setPartC(String partC) {
  		this.partC = partC;
  	}
  
  }
  ```


- 抽象建造者类[也可为接口]  Builder.java
  
  ``` java
  /**
   * 抽象建造者
   */
  public abstract class Builder {
  	protected Product product = new Product();
  	
  	protected abstract void buildPartA();
  	
  	protected abstract void buildPartB();
  	
  	protected abstract void buildPartC();
  	
  	public Product getProduct(){
  		return product;
  	}
  
  }
  ```


- 具体的复杂产品的建造者类 ConcreteBuilder.java
  
  ``` java
  /**
   * 具体的建造者对象<br>
   * 建造复杂产品的各个组成部件、最后由指挥者类<Director>进行组装成完整的产品对象
   */
  public class ConcreteBuilder extends Builder {
  
  	@Override
  	protected void buildPartA() {
  		product.setPartA("build Part A");
  		System.out.println("正在建造产品部件A");
  	}
  
  	@Override
  	protected void buildPartB() {
  		product.setPartB("build Part B");
  		System.out.println("正在建造产品部件B");
  	}
  
  	@Override
  	protected void buildPartC() {
  		product.setPartC("build Part C");
  		System.out.println("正在建造产品部件C");
  	}
  
  }
  ```


- 复杂产品的组装 指挥者类: Director.java
  
  ``` java
  /**
   * 指挥者<br>
   * 负责 安排/组装 复杂对象的建造次序
   */
  public class Director {
  	private Builder builder;
  	
  	public Director(Builder builder){
  		this.builder = builder;
  	}
  	
  	
  	/**
  	 * 产品对象的构建与组装
  	 * @return
  	 */
  	public Product construct(){
  		System.out.println("--- 指挥者开始 构建产品 ---");
  		builder.buildPartA();
  		builder.buildPartB();
  		builder.buildPartC();
  		System.out.println("--- 指挥者 构建产品 完成 ---");
  		return builder.getProduct();
  	}
  
  }
  ```


- 客户端调用测试类: Client.java
  
  ``` java
  public class Client {
  
  	public static void main(String[] args) {
  
  		ConcreteBuilder cBuilder = new ConcreteBuilder();
  		Director director = new Director(cBuilder);
  		director.construct();
  	}
  
  }
  ```



 运行效果

``` java
--- 指挥者开始 构建产品 ---
正在建造产品部件A
正在建造产品部件B
正在建造产品部件C
--- 指挥者 构建产品 完成 ---
```

## 其他

在建造者模式中，客户端不必知道产品对象内部组成的细节，将产品本身与产品的创建过程解耦；每一个具体的建造者都相对独立，可以方便修改/新增具体的建造者而不会对其他的建造者造成影响、使系统扩展方便，符合“开闭原则”。