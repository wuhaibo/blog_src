---
title: RSI 指標
description: 學習RSI 指標
date: 2021-06-23 16:39:28
updated: 2021-06-23 16:39:28
categories:
  - 投資
comments: true
---
## RSI簡介

RSI（relative strength index）這個指標，比較一段時期内的平均收盤漲幅和平均收盤跌幅來分析市場走勢

屬於**動量**指標

### 計算

RSI  = EMA(u,n)/(EMA(u,n) + EMA(d,n)) 

EMA(u,n): n天平均漲幅

EMA(d,n): n天平均跌幅

### 策略

* 儅RSI ＞50 屬於買方强勢，RSI ＜ 50 屬於賣方强勢
* 策略可定為儅RSI＞一個閾值（一般70），賣

  策略可定為儅RSI＜一個閾值（一般30），買
* 閾值一般根據不同股票的特性而不同，一般波動比較大的股票取20，80

  ```python
      
      def get_buy_sig(self):
          
          if self.rsi[0] < 30 and self.rsi[-1] > 30:
              return True
          
          return False

      def get_sell_sig(self):

          if self.rsi[0] > 70 and self.rsi[-1] < 70:
              return True

          return False
  ```



針對納斯達克100的股票進行測試，結果在這裏https://docs.google.com/spreadsheets/d/1tK3nWFvf8Hd2K7yMbgOYC6Bc-I62ZUGoMORf2akz0zU/edit#gid=0