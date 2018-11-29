---
layout: post
title: angular echarts ngx-echarts采坑实录
categories: 前端
description: 没有
keywords: js
---

# angular echarts ngx-echarts使用记录
项目中要使用地图，自然而然想到echarts的map，不过使用过程中遇到大坑，网上没有找到合适的解答，后来找到一个可行的解决方案。

对应错误：在angular-cli中使用ngx-echarts显示echarts的地图组件失败，一片空白.

echarts是百度的一个图表展示的开源工具，方便好用并且十分强大。

## 纯js使用方法

## angularjs项目中使用echarts
在angular/cli中使用echarts时，纯js的方法不太适用，因此我们可以选择针对angular的echarts工具。常见的有echarts-ng2和ngx-echarts，后者使用更为方便、强大。
### 安装
```shell
# if you use npm
npm install echarts --save
# if your angular version >=6
npm install ngx-echarts --save
# else if your angular version<6
npm install ngx-echarts@2.3.1 --save  
```
把这两种工具安装到项目中去。注意一下版本匹配问题，angular版本大于等于6的用户直接运行这条命令就行了，如果版本低于6，需要安装v2.3.1的ngx-echarts。

### 引入
```javascript
+import { NgxEchartsModule } from 'ngx-echarts';

@NgModule({
  imports: [
    ...,
+   NgxEchartsModule
  ],
})
export class AppModule { }
```
在app.module.ts文件中，增加ngx-echarts的导入，这样在后续ts文件中都可以正常使用。

```javascript
{
  ...,
  "scripts": [
+   "node_modules/echarts/dist/echarts.min.js"
  ],
  ...,
}
```
.angular-cli.json中也要把echarts.min.js放进来。注意这个地方，后面还会用到。

### 使用echarts map

echarts的基本组件比较容易，在html中加入一个放置图标的标签就可以了。
```html
<div echarts [options]="options" style="height: 400px; width: 400px"></div>
```
同时还需要在ts文件中加入相关的控制代码。
```javascript
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';

import * as echarts from 'echarts';

declare const require: any;

@Component({
  selector: 'app-hongkong-pd',
  templateUrl: './hongkong-pd.component.html',
  styleUrls: ['./hongkong-pd.component.scss']
})
export class HongkongPdComponent implements OnInit {
  demo_html = require('!!html-loader!./hongkong-pd.component.html');
  demo_ts = require('!!raw-loader!./hongkong-pd.component.ts');

  // empty option before geoJSON loaded:
  options = {};

  constructor(private http: HttpClient) { }

  ngOnInit() {
    // fetch map geo JSON data from server
    this.http.get('assets/data/HK.json')
      .subscribe(geoJson => {
        // register map:
        echarts.registerMap('HK', geoJson);
        // update options:
        this.options = {
          title: {
            text: '香港18区人口密度 （2011）',
            subtext: '人口密度数据来自Wikipedia',
            sublink: 'http://zh.wikipedia.org/wiki/%E9%A6%99%E6%B8%AF%E8%A1%8C%E6%94%BF%E5%8D%80%E5%8A%83#cite_note-12'
          },
          tooltip: {
            trigger: 'item',
            formatter: '{b}<br/>{c} (p / km2)'
          },
          toolbox: {
            show: true,
            orient: 'vertical',
            left: 'right',
            top: 'center',
            feature: {
              dataView: { readOnly: false },
              restore: {},
              saveAsImage: {}
            }
          },
          visualMap: {
            min: 800,
            max: 50000,
            text: ['High', 'Low'],
            realtime: false,
            calculable: true,
            inRange: {
              color: ['lightskyblue', 'yellow', 'orangered']
            }
          },
          series: [
            {
              name: '香港18区人口密度',
              type: 'map',
              mapType: 'HK', // map type should be registered
              itemStyle: {
                normal: { label: { show: true } },
                emphasis: { label: { show: true } }
              },
              data: [
                { name: '中西区', value: 20057.34 },
                { name: '湾仔', value: 15477.48 },
                { name: '东区', value: 31686.1 },
                { name: '南区', value: 6992.6 },
                { name: '油尖旺', value: 44045.49 },
                { name: '深水埗', value: 40689.64 },
                { name: '九龙城', value: 37659.78 },
                { name: '黄大仙', value: 45180.97 },
                { name: '观塘', value: 55204.26 },
                { name: '葵青', value: 21900.9 },
                { name: '荃湾', value: 4918.26 },
                { name: '屯门', value: 5881.84 },
                { name: '元朗', value: 4178.01 },
                { name: '北区', value: 2227.92 },
                { name: '大埔', value: 2180.98 },
                { name: '沙田', value: 9172.94 },
                { name: '西贡', value: 3368 },
                { name: '离岛', value: 806.98 }
              ],
              nameMap: {
                'Central and Western': '中西区',
                'Eastern': '东区',
                'Islands': '离岛',
                'Kowloon City': '九龙城',
                'Kwai Tsing': '葵青',
                'Kwun Tong': '观塘',
                'North': '北区',
                'Sai Kung': '西贡',
                'Sha Tin': '沙田',
                'Sham Shui Po': '深水埗',
                'Southern': '南区',
                'Tai Po': '大埔',
                'Tsuen Wan': '荃湾',
                'Tuen Mun': '屯门',
                'Wan Chai': '湾仔',
                'Wong Tai Sin': '黄大仙',
                'Yau Tsim Mong': '油尖旺',
                'Yuen Long': '元朗'
              }
            }
          ]
        };
      });
  }

}
```
这里给出的例子是ngx-echarts官方demo，实际操作的时候，我遇到了比较大的困难。这里通过网络请求获取到对应想要显示地区的地图json文件。然后把这个json当作一个object对象参数传递到echarts.registerMap方法中去。（地图使用之前必须使用这个函数注册，第一个参数是对该地图的命名，后续option中mapType的值必须和注册时的名字一样）但我自己这么尝试的时候始终无法显示出地图，该区域一片空白。

### 解决方案
除了自己进行注册之外，我们也可以引入包含注册过程的js文件，这样调用的时候直接修改options就能成功显示了。
在我们echarts的安装目录中，有一个map文件夹，其中包含了一些基本的js。
```javascript
//path: node_modules\echarts\map\js\china.js
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD. Register as an anonymous module.
        define(['exports', 'echarts'], factory);
    } else if (typeof exports === 'object' && typeof exports.nodeName !== 'string') {
        // CommonJS
        factory(exports, require('echarts'));
    } else {
        // Browser globals
        factory({}, root.echarts);
    }
}(this, function (exports, echarts) {
    var log = function (msg) {
        if (typeof console !== 'undefined') {
            console && console.error && console.error(msg);
        }
    }
    if (!echarts) {
        log('ECharts is not Loaded');
        return;
    }
    if (!echarts.registerMap) {
        log('ECharts Map is not loaded')
        return;
    }
    echarts.registerMap('china', {**********});
}));
```
代码中星号的实际内容就应该是对应地区的json对象。所以实际上，我们只要把自己想显示的地区的json换到这里，并且给一个名字就可以。
比如
```javascript
echarts.registerMap('xingtai', {"type":"FeatureCollection","features":..........................});
```
在后面填充邢台市的json数据，这样js文件的构造工作就完成了。然后再引入该js。
```javascript
{
  ...,
  "scripts": [
+   "node_modules/echarts/dist/echarts.min.js",
+   "../node_modules/echarts/map/js/china.js",
+   "../node_modules/echarts/map/js/xingtai.js",
  ],
  ...,
}
```
最后直接在ts中构造配置参数options就可以了。json文件可以自己搜索下载，echarts官网目前因为测绘法的问题不提供下载了。

```javascript
import { Component, OnInit } from '@angular/core';

declare const require: any;

@Component({
  selector: 'app-hongkong-pd',
  templateUrl: './hongkong-pd.component.html',
  styleUrls: ['./hongkong-pd.component.scss']
})
export class HongkongPdComponent implements OnInit {
  demo_html = require('!!html-loader!./hongkong-pd.component.html');
  demo_ts = require('!!raw-loader!./hongkong-pd.component.ts');

  // empty option before geoJSON loaded:
  options = {};

  constructor() { }

  ngOnInit() {
    
      this.options = {
          title: {
            text: '中国地图',
            subtext: '',
          },
          tooltip: {
            trigger: 'item',
            formatter: '{b}<br/>{c} (p / km2)'
          },
          toolbox: {
            show: true,
            orient: 'vertical',
            left: 'right',
            top: 'center',
            feature: {
              dataView: { readOnly: false },
              restore: {},
              saveAsImage: {}
            }
          },
          visualMap: {
            min: 800,
            max: 50000,
            text: ['High', 'Low'],
            realtime: false,
            calculable: true,
            inRange: {
              color: ['lightskyblue', 'yellow', 'orangered']
            }
          },
          series: [
            {
              name: '邢台',
              type: 'map',
              mapType: 'china', // map type should be registered
              itemStyle: {
                normal: { label: { show: true } },
                emphasis: { label: { show: true } }
              },
              data: [
              ]
            }
          ]
        };
  }
}
```