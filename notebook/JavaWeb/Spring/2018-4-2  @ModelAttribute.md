---
title: 2018-4-2  @ModelAttribute
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---
SpringMVC每次调用请求处理方法时,都会创建一个Model实例,若想使用它,就要在方法中添加一个Model类型的参数.

所述@ModelAttribute注释可以被用于@RequestMapping 方法的参数来创建或从模型访问对象并将其绑定到该请求。@ModelAttribute也可以用作控制器方法的方法级注释，其目的不是处理请求，而是在请求处理之前添加通常需要的模型属性。
### 用于将带注解的参数创建到Model对象中
```java
@RequestMapping(method=RequestMethod.POST)
public String submitOrder(@ModelAttribute("newOrder")Order order){}
```
### 用于处理方法前调用@ModelAttribute方法
控制器可以有多种@ModelAttribute方法。所有这些方法@RequestMapping在相同控制器中的方法之前被调用。一个@ModelAttribute 方法也可以通过控制器共享@ControllerAdvice。

一个示例@ModelAttribute方法：
```java
@ModelAttribute
public void populateModel(@RequestParam String number, Model model) {
    model.addAttribute(accountRepository.findAccount(number));
    // add more ...
}
```
若方法返回void,则需要自行将实例添加到model中.

https://blog.csdn.net/catoop/article/details/51171108