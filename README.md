jtmpl
=====

Javascript front-end templates, v1.0.1 version

###介绍：
jtmpl是一个简单的前端编译模板，一共只有200行，版本V1.0.1。
原则上是不用再学一门语法，只用原生的js的语法即可。
但为了操作方便内置了部分@语法。

本次更新，替换了原来的group和include语法，添加了each循环输出语法。

使用方法介绍：
----

###一、简单模板直接编译：

模板样本， type="text/jtmpl"：

    <script id="friends" type="text/jtmpl">
      <dl>
      {%for (var f=0,flen=friends.length; f<flen; f++){ %}
        <dt>{%=friends[f].name%}</dt>
        <dd>{%=friends[f].age%}</dd>
        {% } %}
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

语法格式：{%@include src="#子模板id"%}

子模板
    
    <script id="header" type="text/jtmpl">
        <hgroup>
            {% for (var j=0,jlen=headers.length; j<jlen; j++) { %}
            <h2>{% h:=headers[j] %}</h2>
            {% } %}
        </hgrop>
    </script>

子模板：

    <script id="footer" type="text/jtmpl">
        <footer><%=info;%></footer>
    </script>
    
父模板：

    <script id="test" type="text/jtmpl">
        {%@include src="#header" %}
    	<ul>
            {%for (var i=0, len = data.length; i<len; i++){ %}
                <li>{%=data[i]%}</li>
            <%}%>
        </ul>
        {%@include src="#footer" %}
    </script>
    
编译输出结果：

    var html = jtmpl.template('#test', {
            headers : ["标题一", "标题二"],
            data : ["内容一", "内容二"],
            info : "模板测试"
    });
    
结果：

    <hgroup>
        <h2>标题一</h2>
        <h2>标题二</h2>
    </hgrop>
    <ul>
      <li>内容一</li>
      <li>内容二</li>
    </ul>
    <footer>模板测试</footer>
    
###四、支持组装模板：

<%group:子模板id, 子模板id, 子模板id%>  可以缩写 <%g:子模板id, 子模板id, 子模板id%>

子模板：

    <script id="a" type="text/jtmpl">
        <p>{%=name%}</p>
    </script>
    <script id="b" type="text/jtmpl">
        <p>{%=age%}</p>
    </script>
    <script id="c" type="text/jtmpl">
        <p>{%=work%}</p>
    </script>
    
编译组装：

    jtmpl.template('{%@groups items="a, b, c"%}', {name : 'mongo', age : 26, work : 'famer'})
    // 输出 <p>mongo</p><p>26</p><p>famer</p>

###五、each循环输出

语法格式：{% @each items as item%} dosomething {% /@each %}

模板：

	<script id='testeach' type="text/x-jtmpl-template">
		<dl>
		{% @each books as book %}
			<dt>书名：{% =book.name %}</dt>
				<dd>
					<p>价格：{% =book.price %}</p>
					<p>描述：{% =book.desc %}</p>
					<ul>
					{% @each book.readers as reader %}
						<li><span>读者名称：{% =reader.name %}</span> <span>读者年龄：{% =reader.age %}</span></li>
					{% /@each %}
					</ul>
				</dd>
		{% /@each %}
		</dl>
	</script>


数据：
	
	var datas_book = {
			books : [
				{
					name : 'Java 经典实例',
					price : '38',
					desc : 	'本书收集了Java开发人员经常遇到的成百个问题的解决方案，涵盖了Java应用的方方面面，堪称讲述Java应用',
					readers : [
						{name : '张三', age : '26'},
						{name : '李四', age : '31'},
						{name : '王二', age : '28'}
					]
				},
				{
					name : 'Java编程思想（第4版）',
					price : '78',
					desc : 	'本书赢得了全球程序员的广泛赞誉，即使是最晦涩的概念，在BruceEckel的文字亲和力和小而直接的编程示例面前也会化解于无形。',
					readers : [
						{name : '朱八', age : '26'},
						{name : '肯六', age : '31'},
						{name : '杜五', age : '28'}
					]
				}
			]	
		}

编译：
	
	jtmpl.render('#testeach', '#eachcon', datas_book);
	
成生：

	<dl>
	    <dt>书名：Java 经典实例</dt>
	    <dd>
	        <p>价格：38</p>
	        <p>描述：本书收集了Java开发人员经常遇到的成百个问题的解决方案，涵盖了Java应用的方方面面，堪称讲述Java应用</p>
	        <ul>
	            <li><span>读者名称：张三</span> <span>读者年龄：26</span></li>
	            <li><span>读者名称：李四</span> <span>读者年龄：31</span></li>
	            <li><span>读者名称：王二</span> <span>读者年龄：28</span></li>
	        </ul>
	    </dd>
	    <dt>书名：Java编程思想（第4版）</dt>
	    <dd>
	        <p>价格：78</p>
	        <p>描述：本书赢得了全球程序员的广泛赞誉，即使是最晦涩的概念，在BruceEckel的文字亲和力和小而直接的编程示例面前也会化解于无形。</p>
	        <ul>
	            <li><span>读者名称：朱八</span> <span>读者年龄：26</span></li>
	            <li><span>读者名称：肯六</span> <span>读者年龄：31</span></li>
	            <li><span>读者名称：杜五</span> <span>读者年龄：28</span></li>
	        </ul>
	    </dd>
	</dl>
