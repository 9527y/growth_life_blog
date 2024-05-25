---
title: pinpoint，HBase数据清理
categories:
  - 运维
tags:
  - usage
date: 2020-05-29 18:29:40.303000
---
[toc]
# HBASE数据清理
pinpoint使用HBASE储存数据，对于HBase进行数据清理。
## HBase shell
进入HBASE目录，执行如下命令，进入shell
```
bin/hbase shell
```
## 查看表信息
查看HBASE表:
```
hbase(main):001:0> list
TABLE                                                                                                                                                          
AgentEvent                                                                                                                                                     
AgentInfo                                                                                                                                                      
AgentLifeCycle                                                                                                                                                 
AgentStatV2                                                                                                                                                    
ApiMetaData                                                                                                                                                    
ApplicationIndex                                                                                                                                               
ApplicationMapStatisticsCallee_Ver2                                                                                                                            
ApplicationMapStatisticsCaller_Ver2                                                                                                                            
ApplicationMapStatisticsSelf_Ver2                                                                                                                              
ApplicationStatAggre                                                                                                                                           
ApplicationTraceIndex                                                                                                                                          
HostApplicationMap_Ver2                                                                                                                                        
SqlMetaData_Ver2                                                                                                                                               
StringMetaData                                                                                                                                                 
TraceV2                                                                                                                                                        
15 row(s) in 0.2120 seconds
```

## 查看表描述
查看表描述，里面有个TTL值，代表数据存储时长
```
hbase(main):002:0> desc 'AgentInfo'
Table AgentInfo is ENABLED                                                                                                                                     
AgentInfo                                                                                                                                                      
COLUMN FAMILIES DESCRIPTION                                                                                                                                    
{ NAME => 'Info', BLOOMFILTER => 'ROW', VERSIONS => '1', IN_MEMORY => 'false', 
KEEP_DELETED_CELLS => 'FALSE', 
DATA_BLOCK_ENCODING => 'PREFIX', 
TTL => '31536000 SECONDS (365 DAYS)', 
COMPRESSION => 'NONE', MIN_VERSIONS => '0', BLOCKCACHE => 'true', BLOCKSIZE => '65536', REPLICATION_SCOPE => '0'}                         
1 row(s) in 0.1630 seconds
```

## 如果修改表信息
1. 停用表，使用命令`disable 'table_name'`

```
hbase(main):003:0> disable 'TraceV2'
0 row(s) in 8.3610 seconds
```
2. 修改表信息；TTL 单位秒。NAME为`desc`命令查看得的信息
```
hbase(main):004:0> alter 'AgentInfo' , {NAME=>'Info',TTL=>'604800'}
Updating all regions with the new schema...
256/256 regions updated.
Done.
0 row(s) in 1.9750 seconds
```
3. 启用表，使用`enable 'table_name'`命令
```
hbase(main):002:0* enable 'TraceV2'
0 row(s) in 28.5440 seconds
```
