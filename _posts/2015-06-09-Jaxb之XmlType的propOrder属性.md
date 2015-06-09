---
layout: post
title: Jaxb之XmlType的propOrder属性
categories:
- jaxb
tags:
- jaxb
---

今天来学习@XmlType注解的propOrder属性,改属性控制Xml节点的顺序

####属性访问(只包括public，不包含private)

当时用属性访问的时候，propOrder的项是和属性相对应的(即对应的setter，getter方法)，而不是和字段对应。

```java 
@XmlRootElement
@XmlType(propOrder={"ID","firstName","lastName"})
public class Customer{
    private String firstName;
    private String last_name;
    private int id;
    
    public String getFirstName(){
        return firstName;
    }
    public void setFirstName(String firstName){
        this.firstName=firstName;
    }
    public String getLastName(){
        return last_name;
    }
    public void setLastName(String last_name){
        this.last_name=last_name;
    }
    public int getID(){
        return id;
    }
    public void setID(int id){
        this.id=id;
    }
    
}
```   

输出结果如下所示：

```xml  
<?xml version="1.0" encoding="UTF-8"?>
<customer>
    <ID>123</ID>
    <firstName>Jane</firstName>
    <lastName>Doe</lastName>
</customer>
```


####字段访问

当时用字段访问的时候，propOrder的项和字段是相对应的。

```java  

@XmlRootElement
@XmlAccessorType(XmlAccessType.FIELD)
@XmlType(propOrder={"id", "firstName", "last_name"})
public class Customer{
 
 private String firstName;
 private String last_name;
 private int id;
 
 public String getFirstName() {
  return firstName;
 }
 
 public void setFirstName(String firstName) {
  this.firstName = firstName;
 }
 
 public String getLastName() {
  return last_name;
 }
 
 public void setLastName(String last_name) {
  this.last_name = last_name;
 }
 
 public int getID() {
  return id;
 }
 
 public void setID(int id) {
  this.id = id;
 }
 
}
```    

输出结果如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<customer>
    <id>123</id>
    <firstName>Jane</firstName>
    <last_name>Doe</last_name>
</customer>
```

总结 ：

·主要学了@XmlType的属性的使用

####参考：

[JAXB's @XmlType and propOrder](http://blog.bdoughan.com/2012/02/jaxbs-xmltype-and-proporder.html)