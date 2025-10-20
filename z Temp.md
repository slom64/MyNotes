```
SELECT CAST(BulkColumn AS VARCHAR(MAX)) as FileContent FROM OPENROWSET(BULK 'C:\temp\sample.txt', SINGLE_BLOB) AS x;
```

```
a83b750679b1789e29e966d06c7e41f7
```