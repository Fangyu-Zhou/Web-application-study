# Web-application-study
## 后端校验
1)	在数据类(e.g. Topic)的成员变量上加入限制条件的注解(@NotBlank)
2)	在后端get请求Controller处用Model类addAttribute方法传递变量
3)	用thymleaf模板在前端th:object拿到后端Model传递的变量
4)	在验证div处拿到变量中的成员变量
5)	前端加入校验信息框

```
<div class="ui negative message" th:if="${#fields.hasErrors('name')}">
  <i class="close icon"></i>
  <p th:errors="*{name}"></p>
</div>
```


## 自定义后端校验
也是用了BindingResult
/*这里是手动向result中添加一些默认没有的错误, 第一个参数是校验的成员变量名, 第二个是自定义的错误名称, 第三个是错误提示信息*/

```
result.rejectValue("name","nameError","This topic is already exist!");
```

## 后端实现流程
1)	首先从service层开始，先写Service接口，再定义一个实现接口的类 ServiceImplement
2)	Service实现类中需要用到与数据库的交互，这时定义DAO层的Repository 接口 ,继承于JpaRepository<T,主键>可以使用其中的方法也方便我们自定义查询方法
3)	定义好DAO层的Repository后回到ServiceImplement注入 @Autowired (别忘了在类前加上@Service注解)
4)	实现ServiceImplement里的所有方法
5)	写Web层的Controller(注意要注入Service接口应用方法)中的get post方法
6)	回到前端Web去渲染，使用框架(Thymleaf)渲染功能


## 接口可以多继承，在DAO层想要实现复杂查询可以使用SpringBoot封装好的接口JpaSpecificationExecutor<T>

## 复杂查询用到了Predicate类，相当于是一个封装好的SQL查询语句

## 复杂查询和非查询pageable分页查询的做法

1）	非查询页面可以一直使用同一个page变量(Json格式的查询返回变量)
2）	复杂查询为了保证每一页都是查询结果必须使用模板的赋值语句(th:attr=””)来进行page变量的传递

## 动态局部刷新
1）	首先在Controller中加入一个search的get请求handler(与之前的保持一致但需要替换方法名和地址) 同时返回值变成一个片段(即模板中的fragment)
2）	在页面中需要重新渲染的标签中加入th:fragment=
3）	不能使用form表单的sumbit形式发送请求，需要用到AJAX请求

## 点击链接在新标签中打开
Target=”_blank”

## 把markdown格式显示在前端页面
1)  需要在后端将content内容转成html格式
2) 	首先要把dependency加入到工程中pom.xml
3)	写Utils类 类中调整实现插件中的方法以便我们自己的工程使用
4)  Service层应用Utils方法
5)	Controller调用Service方法

## Spring配置MySql数据库
用到很多注解，(e.g. @ManyToMany, @ManyToOne, @OneToMany…etc)这些注解可以直接配置数据库包括主键 外键 是否可为空等等

