---
title: macd
description: 學習macd指標
date: 2021-06-20 20:34:26
updated: 2021-06-20 20:34:26
tags:
  - macd
categories:
  - 投資
comments: true
---
# MACD 學習

## MACD 簡介

### MACD 計算

* **差離值：DIF**值

  DIF = EMA(close,12) - EMA(close,26)
* 訊號綫：DEM或者**MACD**

  DEM = EMA(dif, 9)
* 直方圖：**Histogram** / bar graph

  BAR/OSC = DIF - DEM/MACD

### MACD 解讀

MACD是一種趨勢分析指標，不宜同時分析不同的市場環境。以下為三種交易訊號： 

* 差離值（DIF值）與訊號線（DEM值，又稱MACD值）相交； 
* 差離值與[零軸](https://zh.wikipedia.org/wiki/%E7%AC%9B%E5%8D%A1%E5%B0%94%E5%9D%90%E6%A0%87%E7%B3%BB)相交； 
* 股價與差離值的背離。 
* 差離值（DIF）形成「快線」，訊號線（DEM）形成「慢線」。若股價持續上漲，DIF 值為正，且愈來愈大；若股價持續下跌，DIF 值為負，且負的程度愈來愈大。 
* 當差離值（DIF）從下而上穿過訊號線（DEM），為買進訊號；相反若從上而下穿越，為賣出訊號。買賣訊號可能出現頻繁，需要配合其他指標（如：RSI、KD）一同分析。 DIF 值與 MACD 值在0軸線上，代表市場為牛市，若兩者皆在0軸線之下，代表市場為熊市。 DIF 值若向上突破 MACD 值及0 軸線，為買進訊號，不過若尚未突破0軸，仍不宜買進；DIF 值若向下跌破 MACD 值及0 軸線，為賣出訊號，不過若尚未跌破0軸，仍不宜賣出

## MACD 買入之直方圖從負值變成正值

主要想知道黨macd的直方圖從負變正的時候是不是一個買入的信號，評判為正確的標準為股價在兩周内有上漲。

代碼如下

```python
# git commit d85cafc0021b3591e314dd88c43ac0bc50b8eb92

# strategy
import backtrader as bt
from datetime import datetime



# Macd 策略
class MacdStrategy(bt.Strategy):
    
    #buy_prices = []
    #sell_prices = []

    buy_date = None
    def __init__(self):
        self.macd = bt.indicators.MACDHisto(self.data)

    def stop(self):
        self.log(f'结束时资产：{self.broker.get_value():.2f}')
        
    def next(self):  

        if self.macd_buy_if_his_positive(self.macd.histo):
            self.buy_with_all_cash()
            self.buy_date = len(self)
        
        if self.sell_after_holding(self.buy_date,10):
           self.close_all(self.data) 
    
    # buy if macd his bigger than 0 
    def macd_buy_if_his_positive(self,his_line):
        if his_line[0] > 0 and his_line[-1] < 0:
                return True
        return False

    def sell_after_holding(self,buy_date,days=10):
        buy_price = None
        cur_date = len(self)

        if buy_date != None:
            buy_price = self.data[buy_date - cur_date]
        
        cur_price = self.datas[0].close


        
        ## 兩周内盈利或者超過兩周都要賣出
        if (buy_price != None) and( ( buy_price < cur_price and cur_date <= buy_date + days) 
            or (cur_date > buy_date + days)) :
            return True
        return False

    def buy_with_all_cash(self):
        size = int((self.broker.get_cash() - 10 )/(1.1 * self.data.close))
        if size > 0:
            self.buy(self.data,size)
    
    def close_all(self, data):
        self.close(data)

    def notify_order(self,order):
        # 如果order为submitted/accepted,返回空
        if order.status in [order.Submitted, order.Accepted]:
            return
        # 如果order为buy/sell executed,报告价格结果
        if order.status in [order.Completed]:
            if order.isbuy(): 
                self.log(f'买入: 基金:{order.data._name}'\
                    f' 价格:{order.executed.price:.2f},'\
                    f' 數目:{int(order.executed.value/order.executed.price)},'\
                    # f' 手续费:{order.executed.comm:.2f}'\
                    f' 日期:{order.data.datetime.date()}')
            else:
                self.log(f'賣出: 基金:{order.data._name}'\
                    f' 价格:{order.executed.price:.2f},'\
                    f' 數目:{int(order.executed.value/order.executed.price)},'\
                    # f' 手续费:{order.executed.comm:.2f}'\
                    f' 日期:{order.data.datetime.date()}')

        elif order.status in [order.Canceled, order.Margin, order.Rejected]:
            self.log('交易失败')
            self.log(order.status == order.Margin)
        self.order = None

    def log(self, txt, dt=None,doprint=False):
        #if self.params.printlog or doprint:
        dt = dt or self.datas[0].datetime.date(0)
        info = f'INFO: {dt.isoformat()},{txt}'
        print(info) 


strategy = MacdStrategy
from_date='2021-01-01'
to_date='2021-06-17'
start_cash = 1000000


############### single run###################
# data_obj = hbRunner.get_data_obj_for_run_from_yahoo('PTON',from_date,to_date)
# r = hbRunner.hbrun(data_obj,strategy,start_cash,plot=True)


############### test all nasdaq stocks ###############
nasdaq_ticker_list = hbDataLoader.get_component_list_df_by_index('^NDX')
counter = 0
win_rate = 0
result_array = []
for ticker in nasdaq_ticker_list.index:
    print(ticker)
    counter+=1
    data_obj = hbRunner.get_data_obj_for_run_from_yahoo(ticker,from_date,to_date)
    r = hbRunner.hbrun(data_obj,strategy,start_cash,plot=False)
    win_rate += r['Win Rate']
    print(f"{r['Win Rate']*100:.2f}%")
    result_array.append({'ticker':ticker,'win_rate':r['Win Rate']})

print(f'avg. win rate:{(win_rate*100/counter):.2f}%')
pd.DataFrame(result_array).to_csv('2021nasdaqWinRate.csv')
```

這個策略對納斯達克100的股票進行了測試，在等權重的基礎上，從2019到2021，其中每年的平均買入正確率都為72%