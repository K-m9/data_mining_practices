# Task1-数据探索与分析

## 学习目标
- 了解赛题的背景信息和赛题要求
- 掌握赛题的数据情况和结果评估方法
- 了解数据探索的理论方法
- 理解数据探索的实践
- 实践资金流入流出预测的数据探索


## 赛题背景与信息
- 蚂蚁金服拥有上亿会员并且业务场景中每天都涉及大量的资金流入和流出，面对如此庞大的用户群，资金管理压力会非常大。
- **在既保证资金流动性风险最小，又满足日常业务运转的情况下**，精确地**预测资金的流入流出情况**变得尤为重要。
- 期望参赛者能够通过对例如余额宝用户的申购赎回数据的把握，精准预测**未来每日**的资金流入流出情况。
- 对货币基金而言，**资金流入意味着申购行为，资金流出为赎回行为** 。

## 赛题数据介绍
- 已知数据：余额宝用户的申购赎回数据（2013年7月-2014年8月；每天；28041位用户，2840421条记录）等信息。共4个表格，分别为：<br>
-- 用户信息表： user_profile_table<br>
-- 用户申购赎回数据表： user_balance_table<br>
-- 收益率表(14个月)： mfd_day_share_interest<br>
-- 拆借利率（皆为年化利率）： mfd_bank_shibor  
- 赛题任务：**预测2014年9月每日申购和赎回的总量。**

## 分析过程
- 工具包和数据导入
- 统计申购总量和赎回总量
- 利用时序图观察数据特点
- 分析周一到周日申购总量和赎回总量的差异
- 每月申购总额与赎回总额的分布特点
- 按天分析申购总额与赎回总额
- 分析节假日及特殊日期
- 分析大额交易
- 分析银行拆解利率与余额宝利率
- 分析用户信息


## 代码学习
- 时间戳的截取([pandas中的官方说明文档](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.dt.html?highlight=dt#pandas.Series.dt))
```python3
import datetime
import pandas as pd
# add tiemstamp to dataset
data_balance['date'] = pd.to_datetime(data_balance['report_date'], format= "%Y%m%d")
data_balance['day'] = data_balance['date'].dt.day
data_balance['month'] = data_balance['date'].dt.month
data_balance['year'] = data_balance['date'].dt.year
data_balance['week'] = data_balance['date'].dt.week
data_balance['weekday'] = data_balance['date'].dt.weekday
```

- groupby分组([groupby详解](https://www.cnblogs.com/Yanjy-OnlyOne/p/11217802.html)、[官方文档](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.groupby.html?highlight=groupby#pandas.DataFrame.groupby))
```python3
# total amount
total_balance = data_balance.groupby(['date'])['total_purchase_amt','total_redeem_amt'].sum()
total_balance.reset_index(inplace=True)
```
```python3
# 统计大额用户与小额用户的日总交易额的区别
total_balance_bigNsmall = data_balance.groupby(['date','big_user'], as_index=False)['total_purchase_amt','total_redeem_amt'].sum()
```
