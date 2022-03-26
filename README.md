# WonderTrader 简明使用手册

***版本 Ver 0.9.1***



**关于WonderTrader开源量化交易框架：**

Wesley 开发者（知乎频道）

https://www.zhihu.com/people/yanguoye/posts																					

WonderTrader（官方github）

https://github.com/wondertrader

------

## 功能简介：

- WonderTrade 开源开发框架基于C++语言开发，支持*windows*和*linux*双平台系统
- 支持国内股票、期货等全品种交易市场
- 策略应用层提供基于'C++'语言的**wtcpp**和Python语言的**wtpy**的两套应用框架
- 提供四种交易引擎，以适应高频与跨周期多因子交易策略场景
- 多账户、多产品团队配置管理方案
- 图形化监控分析控制台
- 风险控制机制
- 高速tick级别回测模块		

# 配置安装：

### 一、wtpy应用框架

**【1】安装 Python（版本3.6以上，32位或64位，windows7或windows10操作系统）**

1. 下载地址：https://www.python.org/downloads/

2. 配置path环境变量

   ​	配置教程：https://jingyan.baidu.com/article/b7001fe1dd1ccc0e7282dd36.html

   ![](image/winpath.png)

3. 安装完成后检查python是否安装成功

   检查方法如下：

   ```
   1、打开cmd，输入python，点击回车
   2、输入import this，欣赏下python之禅
   3、输入pip list，检查安装了哪些第三方的安装包
   4、输入exit()，退出python
   ```

4. 配置pip国内镜像源

   ```
   c:>\pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
   ```

   其它镜像源：

   ​	https://mirrors.aliyun.com/pypi/simple/ 

5. 安装wtpy支持包

   ```
   c:>\pip install wtpy --upgrade
   c:>\pip install itsdangerous==2.0.1
   ```

6. 以下命令查看wtpy的版本号

   ```
   C:>\pip show wtpy
   ```

**【2】下载WonderTrader量化开发框架**

1. 下载wtpy应用框架

   ​        github地址：https://github.com/wondertrader/wtpy

   ​           gitee地址：https://gitee.com/wondertrader/wtpy

2. 解压安装目录

   ```
   d:>\wondertrader\wtpy
   ```

   ![](image/wtpydir.png)	

**【3】启动wtpy数据行情应用（上期期货仿真交易环境 ）**

1. 注册仿真账号

   **simnow**注册网址：*http://www.simnow.com.cn*

   ​	*第一次使用仿真账号需更改一次账号密码*

2. 配置数据行情机

   用文本编辑工具打开：***d:>\wondertrader\wtpy\demos\\datakit_fut\mdparsers.YAML***

   添加账号信息保存后退出（行情前置不检测账号信息，改用期货公司行情前置地址保证数据稳定）

   ![](image/md.png)

3. 启动行情机

   执行文件所在目录：***demos\\datakit_fut***

   启动执行： **runDT**

**【4】启动wtpy策略交易应用**

1. 配置**CTA**引擎策略交易

   用文本编辑工具打开：***d:>\wondertrader\wtpy\demos\\cta_fut\tdtraders.YAML***

   添加账号信息保存后退出（交易地址是仿真前置地址，登录时需要验证账号信息）

   ![](image/td.png)

2. 启动策略应用

   拷贝wtpy目录到cta_fut目录；

   执行文件所在目录：***demos\\cta_fut***

   启动执行：**run**
   
   

### 二、wtcpp应用框架

1. 下载wtcpp应用框架

   ​        github地址：https://github.com/wondertrader/wondertrader

   ​        gitee地址：https://gitee.com/wondertrader/wondertrader

2. 解压安装目录

   ```
   d:>\wondertrader\wtcpp
   ```

   ![](image/wtcppdir.png)

3. 配置数据行情机

   用文本编辑工具打开配置文件：***d:>\wondertrader\wtcpp\dist\QuoteFactory\mdparsers.YAML***

   添加账号信息保存后退出（行情前置不检测账号信息，改用期货公司行情前置地址保证数据稳定）

   ![](image/mdcpp.png)

4. 启动行情机

   执行文件所在目录：***dist\\QuoteFactory***

   启动执行： **QuoteFactory**

5. 配置CTA策略交易应用

   用文本编辑工具打开：***d:>\wondertrader\wtcpp\dist\\WtRunnerCta\\tdtraders.YAML***

   添加账号信息保存后退出（交易地址是仿真前置地址，登录时需要验证账号信息）

   ![](image/tdrcpp.png)

6. 启动策略应用

   执行文件所在目录：***dist\\WtRunnerCta***

   启动执行：**WtRunner**

### 三、WonderTrader配置参数说明

1. 
2. 

#### 使用注意事项：

```
1、行情机执行启动时间早于开盘时间2分钟
2、行情机关闭时间晚于16：00
3、启动行情机后再启动CTA引擎策略交易应用
4、编辑YAML文件只允许使用空格
5、自定义订阅品种注意品种名称格式
```



# 交易回测：

1. wtpy应用框架
2. wtcpp应用框架

# 策略实现：

### 一、CTA引擎

#### 【1】wtpy应用交易策略

1. DualThrust策略

2. MACD策略

   ```python
   import pandas as pd
   
   from wtpy import BaseCtaStrategy
   from wtpy import CtaContext
   import numpy as np
   import math
   
   class MACDStra(BaseCtaStrategy):
       
       def __init__(self, name:str, code:str, barCnt:int,
                    period:str, margin_rate:float, money_pct:float, capital, k1:float,k2:float,days:int):
           BaseCtaStrategy.__init__(self, name)
   
           self.__period__ = period
           self.__bar_cnt__ = barCnt
           self.__code__ = code
           self.__margin_rate__ = margin_rate # 保证金比率
           self.__money_pct__ = money_pct # 每次使用的资金比率
           self.__capital__ = capital
           self.today_entry = 0  # 限制每天开仓次数的参数
           self.__k1__ = k1  # 短EMA的天数
           self.__k2__ = k2  # 长EMA的天数
           self.__days__ = days  #
   
       def on_init(self, context:CtaContext):
           code = self.__code__    # 品种代码
   
           context.stra_get_bars(code, 'd1', self.__bar_cnt__, isMain=False)
           context.stra_get_bars(code, self.__period__, self.__bar_cnt__, isMain = True)
           context.stra_log_text("MACDStra inited")
           pInfo = context.stra_get_comminfo(code)
           self.__volscale__ = pInfo.volscale
   
   
       def on_session_begin(self, context:CtaContext, curTDate:int):
           self.trade_next_day = 2
   
       def on_calculate(self, context:CtaContext):
           code = self.__code__    #品种代码
           # 把策略参数读进来，作为临时变量，方便引用
           curPrice = context.stra_get_price(code)
           margin_rate = self.__margin_rate__
           money_pct = self.__money_pct__
           volscale = self.__volscale__
           capital = self.__capital__
           days = self.__days__
           k1 = self.__k1__
           k2 = self.__k2__
           trdUnit_price = volscale * margin_rate * curPrice
           curPos = context.stra_get_position(code)
           if curPos == 0:
               self.cur_money = capital + context.stra_get_fund_data(0)
           df_bars = context.stra_get_bars(code, 'd1', self.__bar_cnt__, isMain=False)
           closes = df_bars.closes
           # 计算MACD
           df = pd.DataFrame(columns=['closes'],data=closes)
           df['EMA_short'] = df['closes'].ewm(span=k1,adjust=False).mean()
           df['EMA_long'] = df['closes'].ewm(span=k2,adjust=False).mean()
           df['DIF'] = df['EMA_short'] - df['EMA_long']
           df['DEA'] = df['DIF'].ewm(span=days, adjust=False).mean()
           df['MACD'] = (df['DIF']-df['DEA']) * 2
           # 获取昨日收盘价
           if df['MACD'].iloc[-1] > 0 and df['DIF'].iloc[-1] > 0 and curPos == 0:
               context.stra_enter_long(code, math.floor(self.cur_money * money_pct / trdUnit_price)
                                       , 'enterlong')
               self.cur_money = capital + context.stra_get_fund_data(0)
               context.stra_log_text('入场做多%s手' % (math.floor(self.cur_money * money_pct / trdUnit_price)))
           elif df['MACD'].iloc[-1] < 0 and df['DIF'].iloc[-1] < 0 and curPos == 0:
               context.stra_enter_short(code, math.floor(self.cur_money * money_pct / trdUnit_price)
                                        , 'entershort')
               self.cur_money = capital + context.stra_get_fund_data(0)
               context.stra_log_text('入场做空%s手' % (math.floor(self.cur_money * money_pct / trdUnit_price)))
           elif df['DIF'].iloc[-1] < 0 and curPos > 0:
               context.stra_set_position(code, 0, 'clear')
               context.stra_log_text('DIF < 0，平多')
           elif df['DIF'].iloc[-1] > 0 and curPos < 0:
               context.stra_set_position(code, 0, 'clear')
               context.stra_log_text('DIF > 0，平空')
           curTime = context.stra_get_time()
           cur_money = capital + context.stra_get_fund_data(code)
           if cur_money < self.cur_money * 0.99 and curPos != 0:
               self.trade_next_day = 0
           if curTime >= 1455 and curTime <= 1500:
               if self.trade_next_day == 0:
                   context.stra_set_position(code, 0, 'clear')
                   context.stra_log_text('亏损超过百分之一，下一交易日平仓')
   ```

#### 【2】wtcpp应用交易策略

1. DualThrust策略
2. MACD策略

### 二、HTF引擎

【1】wtpy应用交易策略

【2】wtcpp应用交易策略

