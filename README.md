jtmpl
=====

Javascript front-end templates, v1.0 version

###介绍：
jtmpl只是一个简单的前端编译模板，初级版本，供自己使用，呆完善。

使用方法介绍：
----

###一、简单模板直接编译：
1 模板样本，type="text/jtmpl"
    <script id="friends" type="text/jtmpl">
      <dl>
      <%for (var f=0,flen=friends.length; f<flen; f++){%>
        <dt><%=friends[f].name%></dt>
        <dd><%=friends[f].age%></dd>
        <%}%>
      </dl>
    </script>


