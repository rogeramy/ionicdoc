##### ngx-echarts

###### 安装和使用

官方步骤，使用添加到最后一层 module.ts，

```
NgxEchartsModule.forRoot({
	echarts:()=>import('echarts'),
})
```



##### 使用

###### 1.折线X轴如果是日期

1. 如果X轴是日期格式，X 轴type设置成'time'，这个时候X轴不需要提供数据源，Y轴数据源为 [X,Y] 格式，X为日期

   ```
   let historyRefuelling = [["2018-08-15T10:04:01.339Z",5],["2018-08-15T10:14:13.914Z",7]]
   
   let historyTheft = [
       ["2018-08-15T10:04:01.339Z",1],[ "2018-08-15T10:14:13.914Z",2],[ "2018-08-15T10:40:03.147Z",3],[ "2018-08-15T11:50:14.335Z",4]
   ]
   
   option =    {
       color: ["#009C95","#21ba45"],
       title : {
           text: 'Fuel History',
           textStyle: {
               fontFamily: 'lato'
           }
       },
       tooltip : {
           trigger: 'axis'
       },
       calculable : true,
       xAxis : [
           {
               type: 'time',
               boundaryGap:false,
               axisLabel: {
                   formatter: (function(value){
                       return moment(value).format('HH:mm');
                   })
               }
           }
       ],
       yAxis : [
           {
               type : 'value'
           }
       ],
       series : [
           {
               backgroundColor: '#4D86FF',
               name:'Refuelling',
               type:'line',
               smooth:true,
               itemStyle: {normal: {areaStyle: {type: 'default'}}},
               data: historyRefuelling
           },
           {
               name:'Fuel Theft',
               type:'line',
               smooth:true,
               itemStyle: {normal: {areaStyle: {type: 'default'}}},
               data: historyTheft
           }
       ]
   }
   ```

   

