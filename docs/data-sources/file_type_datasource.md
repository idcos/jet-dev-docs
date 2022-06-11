---
sidebar_position: 2
sidebar_label: '文件类型数据源'
title: '文件类型数据源'
---

通过提供路径和身份验证信息，从本地源、远程网络服务器或其他实例导入文件。

对于文件类型导入集，您可以从文件检索方法列表中进行选择：SFTP、FTP、FTPS、HTTP、HTTPS 和 SCP。
> 不推荐使用 FTPS，因为某些防火墙供应商不再支持它。考虑使用 SFTP 作为替代方案。

| 方法 | 描述 |
| :------------- | :---------- |
| 附件 |   centered   |
| SFTP        |    使用 SFTP 检索文件。为服务器名称和登录凭据提供了字段。     | 
| FTP |   从网络中的 FTP 服务器检索文件。为服务器名称和登录凭据提供了字段。(FTP 传输以明文形式发送，无法加密。尽可能使用 SCP 或 SFTP。)   |
| FTPS (Auth SSL) [不推荐] |   不推荐，因为某些防火墙供应商不再支持 FTPS。考虑使用 SFTP 作为替代方案。通过安全套接层 (SSL) 协议发出的 FTP 安全认证命令。此方法也称为基于 SSL 的显式 FTP。   |
| FTPS (Auth TLS) [不推荐] |   不推荐，因为某些防火墙供应商不再支持 FTPS。考虑使用 SFTP 作为替代方案。通过传输层安全 (TLS) 协议发出的 FTP 安全认证命令。此方法也称为基于 TLS 的显式 FTP。   |
| HTTP |   使用 HTTP 检索文件。为服务器名称和登录凭据提供了字段。(对于 HTTP 和 HTTPS 协议，文件路径会自动进行 URL 编码。使用这些协议中的任何一个时，不要指定 URL 编码的文件路径。)   |
| HTTPS |   使用 HTTPS 检索文件。为服务器名称和登录凭据提供了字段。使用此方法在 ServiceNow 实例之间传输数据。   |
| SCP |   安全复制协议 (SCP) 使用安全外壳 (SSH) 协议在本地和远程主机之间或两个远程主机之间安全地传输文件。   |


## Excel 数据源文件
您可以使用 XLSX 格式和旧 XLS 格式进行导入。XLS 是在 Excel 2003 及更早版本中创建的电子表格的默认格式。XLSX 是 Excel 2007 或更高版本的默认格式。尽可能使用 XLSX 而非旧版 XLS 以获得最佳性能。
> - 所有 .xls 文件都必须使用 1900 日期系统，而不是 1904 日期系统。1904 日期系统使您的日期被导入的年份比电子表格中显示的日期早四年。有关日期系统的更多信息，请参阅 MS 支持。
> - 从 Excel 中导入大写布尔值为 (TRUE/FALSE) 的文件时，这两个值始终转换为 false。要防止这种行为，请将 glide.transform.boolean.casesensitive属性设置为 false。

##### Excel 演示数据
![etl](/img/IntegrationHub-ETL/etl_excel_demo.png)