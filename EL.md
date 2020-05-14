## EL

表达式语言，可以替代jsp页面的Java代码

servlet（增加数据）-》el中显示数据

将对象放进request域中：(改servlet需要重启，出问题可能是缓存问题clean一下)

#### servlet示例

```java
//一些定义和赋值操作
request.setAttribute("student",student);    
request.getRequestDispatcher("el.jsp").forward(request,response);
```

再跳转到jsp页面，直接在jsp页面获取

#### 普通套用Java代码示例

```jsp
<body>
    <%
    	Student student = (Student)request.getAttribute("student");//强制转换
    	out.print(student);
    	out.print(student.getId());
    %>
</body>
```

传统的在JSP中用Java代码显示的弊端：类型转换、需要处理null、代码参杂

#### el示例：

${域对象.域对象中的属性.属性.属性.级联属性}

#### EL操作符：

​	点操作符 . (key)            	---使用方便

​	中括号操作符[]    		 （和点操作符基本等价 .key==['key']=["key"]）	

​			---功能强大：可以包括特殊字符（、 - ）可以访问数组，可以放变量，加引号是常量，不加引号是变量，

```jsp
${requestScope.student}<br><!--requestScope表示在request域中取值-->
${requestScope.student.id}<br>
${requestScope.student.address.homeAddress.x.x.x}<br>
<!------[""]或者['']操作符------><br>
${requestScope.student.address["homeAddress"]}<br>
<!--String name="student"--><br>
${requestScope.student.address[name]}<br>
<!--String[] array=new String[]{"aaa","ddd"}--><br>
${requestScope.[array[0]}<br>
```

#### EL获取map属性 

```java
Map<String,Object> map=new HashMap<>();
map.put("cn","中国");
map.put("us","美国");
```

```jsp
${requestScope.map.cn}<br>
${requestScope.map["us"]}<br>
```

#### EL运算符

<img src="C:%5CUsers%5C20120%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5C1589338086228.png" alt="1589338086228" style="zoom:80%;" />

```jsp
${3>2 || 3<2 }<br>
${3 gt 2}<br><!--结果为true-->
```

#### Empty运算符

判断一个值null、不存在（true）

#### EL表达式的隐式对象

不需要new就能使用的对象，自带对象

jsp：request\response

##### a.作用域访问对象（EL域对象）

​	pageScope <  requestScope  <  sessionScope  < applicationScope

​	如果不指定域对象，则默认会根据从小到大的顺序依次取值

##### b.参数访问对象

​	获取表单数据（超链接中传值 a.jsp?a=b&c=d,地址栏 a.jsp?a=b&c=d）

​		（request.getParament()(拿单个值)、request.getParamentValues()(拿多个值)）

​								${param}							${paramValues}

##### c.JSP隐式对象	

​	pageContext:在jsp中可以通过pageContext获取其他的jsp隐式对象，因此要在EL中使用隐式对象，就可以通过	pageContext间接获取

​	 例如 		${pageContext.getSession}---->${pageContext.session}  ${pageContext.request}

​	可以使用此方法级联获取方法

​		${pageContext.request.getServerPort}----->${pageContext.request.serverPort}//拿端口号









