---
title: 'IDEA开启 Run DashBoard'
categories:
  - 开发
tags:
  - idea
date: 2020-06-01 23:59:54
---

打开项目内的.idea\workspace.xml，查找RunDashboard。如果不存在，则新建component节点：
```xml
  <component name="RunDashboard">
    <option name="configurationTypes">
      <set>
        <option value="SpringBootApplicationConfigurationType" />
      </set>
    </option>
    <option name="ruleStates">
      <list>
        <RuleState>
          <option name="name" value="ConfigurationTypeDashboardGroupingRule" />
        </RuleState>
        <RuleState>
          <option name="name" value="StatusDashboardGroupingRule" />
        </RuleState>
      </list>
    </option>
```
如果存在，则在节点内增加：
```xml
<option name="configurationTypes">
  <set>
    <option value="SpringBootApplicationConfigurationType" />
  </set>
</option>
```
保存即可。

