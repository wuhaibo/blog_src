---
title: RSI 策略
cover: ""
description: "學習RSI "
date: 2021-06-23 19:07:46
updated: 2021-06-23 19:07:46
tags:
  - RSI
categories:
  - 投資
comments: true
---
## 簡介

RSI（relative strength index）相對强弱指數，是通过比较一段时期内的平均收盘涨数和平均收盘跌数来分析市场买沽盘的意向和实力，从而作出未来市场的走势。



## 計算



RSI＝[**上升平均数**÷(**上升平均数**＋**下跌平均数**)]×100

**上升平均数：**是在某一段日子里升幅数的平均

**下跌平均数：** 是在同一段日子里跌幅数的平均。



### 策略

* 强弱指标保持**高于**50表示为**强勢**市場\
  强弱指标保持**低于**50表示为**弱勢**市場
* 每种类型股票的超卖超买值是**不同**的
* 策略

  ＞70賣

  ＜30買

  ```python
      def get_buy_sig(self):
          
          if self.rsi[0] < 30 and self.rsi[-1] > 30:
              return True
          
          return False

      def get_sell_sig(self):

          if self.rsi[0] > 70 and self.rsi[-1] < 70:
              return True

  ```

  該策略對納斯達克100股票進行了測試，結果在https://docs.google.com/spreadsheets/d/1tK3nWFvf8Hd2K7yMbgOYC6Bc-I62ZUGoMORf2akz0zU/edit#gid=0