jtmpl
=====

Javascript front-end templates, v1.0 version

使用方法介绍：
----

###一、简单模板直接编译：
    <script id="friends" type="text/jtmpl">
      <dl>
      <%for (var f=0,flen=friends.length; f<flen; f++){%>
        <dt><%=friends[f].name%></dt>
        <dd><%=friends[f].age%></dd>
        <%}%>
      </dl>
    </script>


