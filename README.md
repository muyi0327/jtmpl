jtmpl
=====

Javascript front-end templates, v1.0 version

###介绍：
jtmpl只是一个简单的前端编译模板，初级版本，供自己使用，待完善。

使用方法介绍：
----

###一、简单模板直接编译：

模板样本， type="text/jtmpl"：

    <script id="friends" type="text/jtmpl">
      <dl>
      <%for (var f=0,flen=friends.length; f<flen; f++){%>
        <dt><%=friends[f].name%></dt>
        <dd><%=friends[f].age%></dd>
        <%}%>
      </dl>
    </script>

传入数据编译：

    var fhtml = jtmpl.template('#friends', {
        friends : [{name:'tom', age : 28}, {name : 'lucy', age : 29}]	
    });

结果输出：

    <dl>
        <dt>tom</dt>
        <dd>28</dd>
        <dt>lucy</dt>
        <dd>29</dd>
    </dl>
    
如果不传入数据，只编译模板并返回一个可接受传入数据的函数。

###二、支持html过滤

<%html:=data%>, 也可以缩写成<%h:=data%>

    var rhtml = jtmpl.template('<%html:=name%>', {name : '<i>lucy</i>'});
    // 输出：&lt;i&gt;lucy&lt;&#47;i&gt;


###三、支持子模板include

<%part:子模板id%> 也可以简写 <%p:子模板id%>

子模板
    
    <script id="header" type="text/jtmpl">
        <hgroup>
            <% for (var j=0,jlen=headers.length; j<jlen; j++) {%>
            <h2><% h:=headers[j]%></h2>
            <%}%>
        </hgrop>
    </script>
    
父模板：

    <script id="test" type="text/jtmpl">
        <%part:header%>
    	<ul>
            <%for (var i=0, len = data.length; i<len; i++){%>
                <li><%=data[i]%></li>
            <%}%>
        </ul>
    </script>
    
