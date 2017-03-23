# MY
MY
<%@ page import="java.sql.*" %>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<table>
<%
Connection conn=null;
ResultSet rs ;
try {
	Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");

	String connUrl="jdbc:sqlserver://localhost:1433;databaseName=DB01";
	conn=DriverManager.getConnection(connUrl, "sa", "sa12345");
	//String querySQL = "SELECT * FROM emp2" ;
	
	String s_pageNow=request.getParameter("pageNow");
	int pageNow=1;
	int totalPage=0;//總頁數
	int pageSize=3; //頁面顯示比數
	int rowCount=0; //總比數
	if(s_pageNow !=null){
		pageNow=Integer.valueOf(s_pageNow);
	}
	String querySQL ="select * from (select row_number() over (order by empno) as RowNum,empno,ename,job from emp2) as NewTable "+
			"where RowNum >"+(pageNow-1)*pageSize+" and RowNum<="+pageNow*pageSize; 
	String countSQL = "SELECT count(*) FROM emp2";
	PreparedStatement stmt = conn.prepareStatement(countSQL);
	//stmt.setString(1, args[0]);
	rs=stmt.executeQuery();
	if(rs.next()) rowCount=rs.getInt(1);
	if(rowCount%pageSize==0)totalPage=rowCount/pageSize;
	else totalPage=(rowCount/pageSize)+1;
	
	for(int i=1;i<=totalPage;i++){
		out.print("<a href=TestPage.jsp?pageNow="+i+"> ["+i+"] </a>");
	}
	out.print("querySQL: "+querySQL);
	 stmt = conn.prepareStatement(querySQL);
	//stmt.setString(1, args[0]);
	rs=stmt.executeQuery();
	while (rs.next()) {
		out.println("<tr>");
		out.println("<td>"+rs.getString(1)+"</td>");
		out.println("<td>"+rs.getString(2)+"</td>");
		out.println("<td>"+rs.getString(3)+"</td>");
		out.println("</tr>");
	}

	} catch (Exception  e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	}
%>
</table>
<select>
  <option>Volvo</option>
  <option selected="selected" value=30>30</option>
  <option value=50>50</option>
  <option value=100>100</option>
  <option value=all>all</option>
</select>

</body>
</html>
