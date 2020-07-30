##### API，函数，语法

###### 1.时间处理

date-fns日期工具

安装：npm install --save date-fns

在angualr5.0项目中只需在使用时引入就好了,不需要在module.ts文件中引入.需要什么函数,就引入什么函数

```
import { isToday, format } from 'date-fns';
```

getDate(): 获取传入的日期是几号，接受Date格式，返回星期数字

```
 getDay(new Date(item.planDay))
```

目前版本不在支持string类型的问题

```
import {format} from 'date-fns'
import { parseISO } from "date-fns/fp";
let date = '2020-07-29T16:00:00.000+0000';
format(parseISO(date), "yyyy-MM-dd HH:mm:ss"); //使用parseISO就可以解决了
```

```
1. isToday():判断所传入日期是否为今天
2.isYesterday(): 判断是否为昨天
3.isTomorrow()判断是否为明天. 用法与isToday(), isYesterday()用法相同,就不加以累述了.
4.format(): 格式化日期函数
5. addDays():获得第n天之后的日期;
6.addHours(): 获得当前小时之后的小时(比如现在5点, 得到七点的时间).
7.addMinutes():获得当前分钟之后n分钟的时间
8.addMonths(): 获得当前月之后n个月的月份
9.subDays():获得当前日期之前n天的日期
10: subHours(): 获得当前时间之前n小时的时间
11:subMinutes(): 获得当前时间之前n分钟的时间
12: subMonths():获得当前月份之前n个月的时间
13: differenceInDays(): 获得两个时间相差几天,
14:differenceInHours();获得两个时间相差的小时.
15: differenceInMinutes(): 获得两个时间相差的分钟
16: differenceInMonths():获得两个时间相差月份
17: differenceInWeeks(): 获得两个时间相差的周数
18: differenceInYears():获得两个时间相差的年数
19:startOfDay():返回传入日期一天开始的Date对象(一天开始的时间)
20: endOfDay(): 获得传入日期一天的结束时间(与startOfDay对应)
21: startOfMonth():获取月份的第一天
22: endOfMonth(): 获得月份的最后一天
23: getDate(): 获取传入的日期是几号;
24: getDay(): 获取传入的日期是星期几
25: getMonth(): 返回传入时间的月份
26: getMinutes(): 返回传入时间的分钟数
27:getHours():返回传入时间的小时数
28: getISOWeek(): 返回传入时间所在月份的第几周．
29: isEqual(): 判断传入的时间是否相等
30:max: 取得时间数组中的最大值
31: min(): 取得时间数组中的最小值
32：目前版本不在支持string类型的问题
```

