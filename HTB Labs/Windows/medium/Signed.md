



```
Database Name                  Owner
------------------------------ -----
master                         sa   
model                          sa   
msdb                           sa   
tempdb                         sa   
Total: 4 database(s)
```

```
SELECT  o.name AS proc_name, USER_NAME(o.principal_id) AS owner_name, OBJECTPROPERTY(o.object_id,'IsExecuteAs') AS is_execute_as_owner, LEFT(m.definition, 4000) AS definition_snippet FROM sys.objects o JOIN sys.sql_modules m ON o.object_id = m.object_id WHERE o.name LIKE 'sp_sysdac_%' ORDER BY o.name;
```

```
SELECT BulkColumn FROM OPENROWSET(BULK 'C:\Path\To\YourFile.txt', SINGLE_CLOB) AS FileContent;
```

```

SET NOCOUNT ON; SELECT name AS principal, master.dbo.fn_varbintohexstr(sid) AS full_sid_hex, LEFT(master.dbo.fn_varbintohexstr(sid), LEN(master.dbo.fn_varbintohexstr(sid)) - 8) AS domain_sid_hex, ( CONVERT(BIGINT, SUBSTRING(sid, DATALENGTH(sid)-3, 1)) + CONVERT(BIGINT, SUBSTRING(sid, DATALENGTH(sid)-2, 1)) * 256 + CONVERT(BIGINT, SUBSTRING(sid, DATALENGTH(sid)-1, 1)) * 256 * 256 + CONVERT(BIGINT, SUBSTRING(sid, DATALENGTH(sid),   1)) * 256 * 256 * 256 ) AS rid FROM sys.server_principals WHERE sid IS NOT NULL AND name LIKE '%\%';

```

```
SET NOCOUNT ON; SELECT member.name AS member_name, role.name AS role_name FROM sys.server_role_members rm JOIN sys.server_principals member ON rm.member_principal_id = member.principal_id JOIN sys.server_principals role   ON rm.role_principal_id   = role.principal_id ORDER BY role_name, member_name;
```

```
SET NOCOUNT ON; SELECT name AS principal, master.dbo.fn_varbintohexstr(sid) AS full_sid_hex, LEFT(master.dbo.fn_varbintohexstr(sid), LEN(master.dbo.fn_varbintohexstr(sid)) - 8) AS domain_sid_hex, ( CONVERT(BIGINT, SUBSTRING(sid, DATALENGTH(sid)-3, 1)) + CONVERT(BIGINT, SUBSTRING(sid, DATALENGTH(sid)-2, 1)) * 256 + CONVERT(BIGINT, SUBSTRING(sid, DATALENGTH(sid)-1, 1)) * 256 * 256 + CONVERT(BIGINT, SUBSTRING(sid, DATALENGTH(sid),   1)) * 256 * 256 * 256 ) AS rid FROM sys.server_principals WHERE sid IS NOT NULL AND DATALENGTH(sid) >= 12 AND name LIKE '%\%' ORDER BY principal;
```

```
S-1-5-21-4088429403-1159899800-2753317549
ef699384c3285c54128a3ee1ddb1a0cc
```