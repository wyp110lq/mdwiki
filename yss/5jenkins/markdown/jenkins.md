# 1.分布式部署项目
##主节点
master端（jenkins主服务）
##从节点
从节点服务器不需要安装jenkins，只需要运行一个slave节点服务，构建事件的分发由master端（jenkins主服务）来执行。


#2.开发代码编译、SonarQube扫描

maven、svn、git、jdk

#3.部署jar或war包至测试环境

批处理脚本(windows)、shell脚本(linux)

#4.执行接口测试
###4.1 ant+jmeter+git+groovy

###4.2 MultiJob Phase配置jmeter测试脚本

#5.oracle数据库操作

5.1同步开发库

5.2备份dmp文件

5.3其他oracle操作，比如：创建用户、授权等基础操作


#6.电子邮件模块

groovy脚本模板：

```
<meta http-equiv="Content-Type" content="text/html;charset=utf-8" />
<STYLE>
BODY, TABLE, TD, TH, P {
  font-family:Verdana,Helvetica,sans serif;
  font-size:11px;
  color:black;
}
h1 { color:black; }
h2 { color:DarkSlateGray; }
h3 { color:black; }
TD.bg1 { color:white; background-color:#0000C0; font-size:120% }
TD.bg2 { color:white; background-color:#4040FF; font-size:300% }
TD.bg3 { color:white; background-color:#8080FF; }
TD.test_passed { color:blue; }
TD.test_failed { color:red; }
TD.console { font-family:Courier New; }

</STYLE>
<BODY>

<script>
document.write("<h1>这是一个标题</h1>");
document.write("<p>这是一个段落。</p>");
</script>

<h2>您好！下面是[eee]接口自动化测试结果,详细报告请登录jenkins查看:</h2> 
<TABLE>
  <TR><TD align="right"><IMG SRC="${rooturl}static/e59dfe28/images/32x32/<%= (build.result == null || build.result.toString() == 'SUCCESS') ? "blue.gif" : build.result.toString() == 'FAILURE' ? 'red.gif' : 'yellow.gif' %>" />
  </TD><TD align="left"><B style="font-size: 200%;">BUILD ${build.result ?: 'SUCCESSFUL'}</B></TD></TR>
  <TR><TD>jenkins地址:</TD><TD><A href="${rooturl}">${rooturl}</A></TD></TR>
  <TR><TD>工程名称:</TD><TD>${project.name}</TD></TR>
  <TR><TD>构建日期:</TD><TD>${it.timestampString}</TD></TR>
  <TR><TD>时长:</TD><TD>${build.durationString}</TD></TR>
  <TR><TD>构建者:</TD><TD><% build.causes.each() { cause -> %> ${cause.shortDescription} <%  } %></TD></TR>
</TABLE>  
<h2>==================================================</h2>   
  <h2>测试报告如下:</h2> 
  
<!-- 判断start：获取失败的数量和成功的数量,初始为0-->

<%
def totalfailure = 0
def totalsuccess = 0
def passingrate = 0.00
%>

<%  build.getSubBuilds().each() { %>

<% if(it.getResult().toString() != 'SUCCESS') { %>
	<% totalfailure += 1 %>
<%  } %>

<% if(it.getResult().toString() == 'SUCCESS') { %>
	<% totalsuccess += 1 %>
<%  } %>

	<% passingrate = totalsuccess/(totalsuccess+totalfailure)*100
	   passingrate = String.format("%.2f", passingrate)
       passingrate = passingrate + "%"
	  	 
	%>

<%  } %>

<h1 style="color:red">失败数量为:${totalfailure}</h1>
<h1 style="color:Coral">成功数量为:${totalsuccess}</h1>
<h1 style="color:Green">通过率:${passingrate}</h1>

<!-- 判断end：获取失败的数量和成功的数量-->


<TABLE border="1px" cellspacing="1" cellpadding="1" width="100%" table-layout:fixed;> 
  <TR>
   <TD width="20" align="left"><B>图标</B></TD>
   <TD width="150" align="left"><B>工程名称</B></TD>
   <TD width="50" align="left"><B>构建状态</B></TD>
   <TD width="50"align="left"><B>测试结果</B></TD>
   <TD width="200"align="left"><B>日志路径</B></TD>
   <TD width="50"align="left"><B>负责人</B></TD>

  </TR>
<h1>以下为测试失败的用例，请处理。</h1>  

<!-- 判断：构建失败的放一个列表-->
<%  build.getSubBuilds().each() { %>
<% if(it.getResult().toString() != 'SUCCESS') { %>
  <TR>
  <TD align="center" width="20"><IMG SRC="${rooturl}static/e59dfe28/images/32x32/<%= (it.getResult().toString() == 'SUCCESS') ? "blue.gif": it.getResult().toString() == 'FAILURE' ? 'red.gif' : 'yellow.gif' %>" /></TD>
  <TD align="left" width="150"><B style="font-size: 100%;">${it.jobName}</B></TD>
  <TD align="left" width="50"><B style="font-size: 100%;">${it.getResult().toString()}</B></TD>
  <TD align="left" width="50"><B style="font-size: 100%"> 测试失败 </B></TD>
  <TD align="left" width="200"><B style="font-size: 100%"> <A href=" ${rooturl}job/${it.jobName}/HTML_20Report">  ${rooturl}job/${it.jobName}/HTML_20Report</A> </B></TD>
  <TD align="left" width="50"><B style="font-size: 100%"><%= 
													it.jobName.contains("123") ? 'wyp'
													: it.jobName.contains("现券卖出") ? 'ls'																									
													: 'ww' %> </B></TD>
													
													
  </TR>  
<%  } %>
<%  } %>
</TABLE> 

<!-- 判断：构建成功的放一个列表 -->
</BR>
<h1>以下为测试通过的用例，无需处理。</h1>
<TABLE border="1px" cellspacing="1" cellpadding="1" width="100%" table-layout:fixed;> 
<TR>
   <TD width="20" align="left"><B>图标</B></TD>
   <TD width="150" align="left"><B>工程名称</B></TD>
   <TD width="50" align="left"><B>构建状态</B></TD>
   <TD width="50"align="left"><B>测试结果</B></TD>
   <TD width="200"align="left"><B>日志路径</B></TD>
   <TD width="50"align="left"><B>负责人</B></TD>

  </TR>
<%  build.getSubBuilds().each() { %>

<% if(it.getResult().toString() == 'SUCCESS') { %>
  <TR>
  <TD align="center" width="20"><IMG SRC="${rooturl}static/e59dfe28/images/32x32/<%= (it.getResult().toString() == 'SUCCESS') ? "blue.gif": it.getResult().toString() == 'FAILURE' ? 'red.gif' : 'yellow.gif' %>" /></TD>
  <TD align="left" width="150"><B style="font-size: 100%;">${it.jobName}</B></TD>
  <TD align="left" width="50"><B style="font-size: 100%;">${it.getResult().toString()}</B></TD>
  <TD align="left" width="50"><B style="font-size: 100%"> 测试通过 </B></TD>
  <TD align="left" width="200"><B style="font-size: 100%"> <A href=" ${rooturl}job/${it.jobName}/HTML_20Report">  ${rooturl}job/${it.jobName}/HTML_20Report</A> </B></TD>
  <TD align="left" width="50"><B style="font-size: 100%"><%= 
													it.jobName.contains("123") ? 'wyp'
													: it.jobName.contains("现券卖出") ? 'ls'																									
													: 'ww' %> </B></TD>																										
  </TR>  
<%  } %>
<%  } %>
</TABLE>  
 
<h3>jenkins查询用户:</h3>  <h3>用户/密码:ysslooklog/ysslooklog</h3>

</BODY>

```