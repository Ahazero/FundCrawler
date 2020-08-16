# 天天基金爬虫
爬取天天基金网上的所有基金，辅助对基金投资的选择

#### 购买基金前，请务必在官方网站上确认爬取的数据无误！

### 坑
- Mac os无法使用，待修复（等到我有钱自己买的时候吧）
- 如果爬取的时候弹了很多无解析方法这样的错误，请在网络情况好的时候重新爬
- 上面这个问题需要到下次重构才能更好地解决
        
## 功能特性
- 爬取基金的近1、3、6月，近1、3年及成立来的收益率，当前基金经理及其任职时间、任职来的收益率及总的任职时间
- 模仿tcp的拥塞避免的线程数量控制，慢开始，当出现错误时，线程最大值减半，成功则线程最大值+1
- 爬取全部数据需要136s，瓶颈为网站的反爬策略

- 结果展示
    ![Image text](./image/result-2.png)
    2019/12/24 共有7571个基金

## 食用方法
- 环境依赖
    运行环境Python3.7
    依赖库requests、eprogress（非必须，若无，则会以一种简陋的方式显示当前进度）
- 下载所有.py脚本文件（除MonkeyTest外）
- 爬取基金数据
    - 运行CrawlingFund.py并等待
- ~~筛选基金~~ 还没做
- ~~一些分析~~ 也没做

## 文件结构

        -CrawlingFund 爬取主文件，描述整个的爬取逻辑并定义了基金信息的数据结构
        -CrawlingWebpage 爬取网页的核心，定义了从输入网页链接到获得html文本的过程
        -FakeUA 提供虚假的UA（将来可能会合并到工具文件）
        -ParsingHtml 负责html文本的解析，以及解析后的动作（保存文件），通过不同的实现类来针对性地对不同的网站内容进行解析
        -ProvideTheListOfFund 负责提供需要爬取的基金列表，包括基金名称、代码
        -MonkeyTest 这是给我自己测试用的
        
## 下次更新内容
- 重构（当前的代码结构实在是太混乱了，增加新的功能需要先等待重构完成。）
- 增加新的爬取字段
  - 标准差
  - 夏普比率
  - 净值 （包括单位、累计和实时估算）
- 增加能按收益率等初步对基金进行筛选的脚本

## 自定义指南
- 虚假UA获取
        
        自定义类，并提供一个实例，可以通过 ins.random来获得一个UA（str）
        替换所有的fake_ua

- 指定爬取的基金列表或获取基金列表的方式

        在ProvideTheListOfFund.py继承GetFundList类并实现_set_fund_list_generator方法
        或者通过其他方法修改属性_fund_list_generator的值
        在CrawlingFund.py中将自定义的类传入crawling_fund方法
        已实现的类：
        GetFundListByWeb 从天天基金网站下载全部基金列表
        GetFundListFromList 传入要爬取的基金列表
        GetFundListFromFile 从文件中获得基金列表  （这个还没做）
        
- 指定爬取方法
        
        继承CrawlingWebpage.py中的GetPage类
        在CrawlingFund.py中的crawling_fund方法里，将爬取核心类替换为自定义的类
        已实现的类：
        GetPageByWebWithAnotherProcessAndMultiThreading 子进程，通过多线程的方式获取页面，模拟tcp的拥塞控制来管理线程数量
        
