<h2>《Spring In Action》 :books: </h2> 

> [美] Craig Walls / Ryan Breidenbach  著    人民邮电出版社

```java
19.定义 Velocity 视图: http://jakarta.apache.org/velocity.
  基于 Velocity 的课程列表: courseList.vm
  <html>
    <head>
      <title>Course List</title>
    </head>
    <body>
      <h2>COURSE LIST</h2>
      <table width="600" border="1" cellspacing="1" cellpadding="1">
        <tr bgcolor="#999999">
          <td>Course ID</td>
          <td>Name</td>
          <td>Instructor</td>
          <td>Start</td>
          <td>End</td>
        </tr>
        #foreach($course in $courses)
        <tr>
          <td><a href="displayCourse.htm?id=${course.id}"> ${course.id} </a></td>
          <td> ${course.name} </td>
          <td> ${course.instructor.lastName} </td>
          <td> ${course.startDate} </td>
          <td> ${course.endDate} </td>
        </tr>
        #end  // 在所有课程中循环
      </table>
    </body>
  </html>

20.配置 Velocity 引擎。
 <bean id="velocityConfigurer" class="org.springframework.web.servlet.view.velocity.VelocityConfigurer">
   <property name="resourceLoaderPath">
      <value>WEB-INF/velocity/</value>
   </property>
   <property name="velocityProperties">
      <props>
        <prop key="directive.foreach.counter.name">loopCounter</prop>
        <prop key="directive.foreach.counter.initial.value">0</prop>
      </props>
   </property>
 </bean>

21.解析 Velocity 视图。
 <bean id="viewResolver" class="org.springframework.web.servlet.view.velocity.VelocityViewResolver">
    <property name="suffix"><value>.vm</value></property>
 </bean>

 return new ModelAndView("courseList", "courses", allCourses);
 
22.格式化日期和数字。
 <bean id="viewResolver" class="org.springframework.web.servlet.view.velocity.VelocityViewResolver">
    …
    <property name="dateToolAttribute">
       <value>dateTool</value>
    </property>
    <property name="numberToolAttribute">
       <value>numberTool</value>
    </property>
  </bean>

  $numberTool.format("000000", course.id)

  $dateTool.format("FULL", course.startDate)
  $dateTool.format("FULL", course.endDate)

23.暴露请求和会话属性。
 <bean id="viewResolver" class="org.springframework.web.servlet.view.velocity.VelocityViewResolver">
    …
    <property name="exposeRequestAttributes">
       <value>true</value>
    </property>
    <property name="exposeSessionAttributes">
       <value>true</value>
    </property>
 </bean>

24.在 Velocity 中绑定表单域。
  在 Velocity 模板中使用 #springBind.
  #springBind("command.phone")
  phone: <input type="text" name="${status.expression}" value="$!status.value">
         <font color="#FF0000">${status.errorMessage}</font><br>

  #springBind("command.email")
  email: <input type="text" name="${status.expression}" value="$!status.value">
         <font color="#FF0000">${status.errorMessage}</font><br>

  #springBindEscaped("command.email", true)

  <bean id="viewResolver" class="org.springframework.web.servlet.view.velocity.VelocityViewResolver">
    …
    <property name="exposeSpringMacroHelpers">
       <value>true</value>
    </property>
  </bean>
```