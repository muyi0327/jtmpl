jtmpl
=====

Javascript front-end templates, v1.0 version

1 直接编译：
  模板
   <script id="friends" type="text/jtmpl">
		<dl>
			<%for (var f=0,flen=friends.length; f<flen; f++){%>
			<dt><%=friends[f].name%></dt>
			<dd><%=friends[f].age%></dd>
			<%}%>
		</dl>
	</script>
	
	编译模板：
	var fhtml = jtmpl.template('#friends', {
			friends : [{name:'tom', age : 28}, {name : 'lucy', age : 29}]	
	});
	
	结果：
	<dl>
    <dt>tom</dt>
    <dd>28</dd>
    <dt>lucy</dt>
    <dd>29</dd>
  </dl>
