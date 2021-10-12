+++
author = "Raikay"
title = "T4模板 生成多个文件 映射整个库表"
date = "2019-03-11"
description = "T4模板 生成多个文件 映射整个库表"
tags = [
    "dotnet",
]

+++

```csharp
<#@ output extension="txt" #>
<#@ template debug="false" hostspecific="true" language="C#" #>
<#@ assembly name="System.Data" #>
<#@ assembly name="System.xml" #>
<#@ import namespace="System.Collections.Generic" #>
<#@ import namespace="System.Data.SqlClient" #>
<#@ import namespace="System.Data" #>
<#@ assembly name="System.Core" #>
<#@ import namespace="System.Linq" #>
<#@ include file="MultiOutput.tt" #>
<#
    string connectionString= "server=.;database=57blogDB;uid=sa;pwd=123;";        
    SqlConnection conn = new SqlConnection(connectionString);
    conn.Open();
    
    string selectQuery ="SET FMTONLY ON; select * from @tableName; SET FMTONLY OFF;";
    SqlCommand command = new SqlCommand(selectQuery,conn);
    SqlDataAdapter ad = new SqlDataAdapter(command);
    System.Data.DataSet ds = new DataSet(); 
    System.Data.DataTable schema = conn.GetSchema("Tables");

    foreach(System.Data.DataRow row in schema.Rows)
    {    
        ds.Tables.Clear();
        string tb_name = row["TABLE_NAME"].ToString();        
        command.CommandText = selectQuery.Replace("@tableName",row["TABLE_NAME"].ToString());
        ad.FillSchema(ds,SchemaType.Mapped,tb_name);  
        string  content=tb_name.ToString();
       // manager.StartNewFile(tb_name+".cs");
#>
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using System.Data;
using System.Data.OleDb;
using System.Configuration;
using System.Data.SqlClient;
using System;
namespace MyProject.Entities
{    

    public class <#= content#>Model      
    {     
      //-------------------------------------------
<#
    string selectQueryt = "select * from "+content;
    string tableName=content;
    System.Data.DataTable schemat = null;
    SqlCommand selectcommandt = new SqlCommand(selectQueryt, conn);
    SqlDataAdapter sdat = new SqlDataAdapter(selectcommandt);
    System.Data.DataSet dsst = new   System.Data.DataSet();
    sdat.Fill(dsst);
    schemat=dsst.Tables[0];
    System.Data.DataTable   dt = dsst.Tables[0];
    foreach (DataColumn dc in dsst.Tables[0].Columns)
    {#>
        private <#= dc.DataType.Name #> _<#= dc.ColumnName.Replace(dc.ColumnName[0].ToString(), dc.ColumnName[0].ToString().ToLower()) #>;
        /// <summary>
        /// <#= dc.ColumnName #>
        /// </summary>   
        public <#= dc.DataType.Name #> <#= dc.ColumnName #>
        {
            get { return _<#= dc.ColumnName.Replace(dc.ColumnName[0].ToString(), dc.ColumnName[0].ToString().ToLower()) #>; }
            set { _<#= dc.ColumnName.Replace(dc.ColumnName[0].ToString(), dc.ColumnName[0].ToString().ToLower()) #> = value; }                              
        } 
  <#}#>
       //-------------------------------------------------
    }   
}
 <# SaveOutput(""+row["TABLE_NAME"].ToString()+".cs"); #>
<#
}
DeleteOldOutputs();
#>
```

