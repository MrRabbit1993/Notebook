# GIS地图相关知识整理

## 一. 地图常用背景知识

### 地图坐标系

| 地图名称 | 简介                                                         |
| -------- | ------------------------------------------------------------ |
| 百度地图 | 1）境内（包括港澳台）：BD09  a、在GCJ-02坐标系基础上再次加密 b、支持WGS-84、GCJ-02转换成BD09，反向不支持，并且批量转换一次有条数限制 |
| 高德地图 | 1）境内：GCJ-02 a、WGS-84——>GCJ-02（高德有接口提供，反过来没有）2）境外：暂不支持 3）AMap 就是高德地图，是高德地图在纳斯达克上市用的名字，主要面向互联网企业或个人提供免费API服务 4）MapABC 是高德集团底下的图盟公司，主要面向大众型企业或政府机关，并提供付费的有偿服务 5）Amap和MapABC，数据和服务都是共享的，所以Mapabc用Amap的API是正常的 |
| 天地图   | 全球统一：CGCS2000                                           |
| 谷歌地图 | 1）境内：GCJ-02  a、数据来源于高德，两者互通 2）境外：WGS-84 |

### GEOJSON
` GeoJSON是一种对各种地理数据结构进行编码的格式，基于Javascript对象表示法的地理空间信息数据交换格式。GeoJSON对象可以表示几何、特征或者特征集合。GeoJSON支持下面几何类型：点、线、面、多点、多线、多面和几何集合。GeoJSON里的特征包含一个几何对象和其他属性，特征集合表示一系列特征。

GeoJSON总是由一个单独的对象组成。这个对象（指的是下面的GeoJSON对象）表示几何、特征或者特征集合。

+ GeoJSON对象可能有任何数目成员（名/值对）
+ GeoJSON对象必须有一个名字为"type"的成员。这个成员的值是由GeoJSON对象的类型所确定的字符串。
+ type成员的值必须是下面之一："Point", "MultiPoint", "LineString", "MultiLineString", "Polygon", "MultiPolygon", "GeometryCollection", "Feature", 或者 "FeatureCollection"。
+ GeoJSON对象可能有一个可选的"crs"成员，它的值必须是一个坐标参考系统的对象。
+ GeoJSON对象可能有一个"bbox"成员，它的值必须是边界框数组。

```
GeoJSON特征集合：
{
    "type": "FeatureCollection",
    "features": [{
            "type": "Feature",
            "geometry": {
                "type": "Point",
                "coordinates": [102.0, 0.5]
            },
            "properties": {
                "prop0": "value0"
            }
        }, {
            "type": "Feature",
            "geometry": {
                "type": "LineString",
                "coordinates": [[102.0, 0.0], [103.0, 1.0], [104.0, 0.0], [105.0, 1.0]]
            },
            "properties": {
                "prop0": "value0",
                "prop1": 0.0
            }
        }, {
            "type": "Feature",
            "geometry": {
                "type": "Polygon",
                "coordinates": [[100.0, 0.0], [101.0, 0.0], [101.0, 1.0], [100.0, 1.0], [100.0, 0.0]]
            },
            "properties": {
                "prop0": "value0",
                "prop1": {
                    "this": "that"
                }
            }
        }
    ]
}
```

~ 详细解释
+ point 点
+ polyline 线
+ polygon 多边形,面
+ Multipoint 点集

### 地图服务

#### WMS
WMS服务：Web Map Service，网络地图服务，它是利用具有地理空间位置信息的数据制作地图，其中将地图定义为地理数据的可视化表现，能够根据用户的请求，返回相应的地图，包括PNG、GIF、JPEG等栅格形式，或者SVG或者WEB CGM等矢量形式。WMS支持HTTP协议，所支持的操作是由URL决定的。
WMS提供如下操作:

+ GetCapabitities：返回服务级元数据，它是对服务信息内容和要求参数的一种描述。
+ GetMap：返回一个地图影像，其地理空间参考和大小参数是明确定义了的。
+ GetFeatureInfo：返回显示在地图上的某些特殊要素的信息。
+ GetLegendGraphic：返回地图的图例信息。

#### WMS-C
WMS-C全称是Web Mapping Service - Cached，对它完整的定义来源于OSGeo Wiki，2006年在FOSS4G会议上提出讨论，目的在于提供一种预先缓存数据的方法，以提升地图请求的速度，自始至终该标准都没有写入OGC之中。WMS-C通过bbox和resolutions去决定请求的地图层级，为了更加直观的请求地图瓦片，一些软件做了一些改进，例如WorldWind在请求中使用level/x/y三个参数，直观明了。典型的基于WMS-C的实现是TileCache，另外一个关于WMSC的参考：http://wiki.osgeo.org/wiki /WMS_Tiling_Client_Recommendation

#### TMS
(tile map Servcie)切片地图服务（TMS）定义了一些操作，这些操作允许用户按需访问切片地图，访问速度更快，还支持修改坐标系。WMTS可能是OGC首个支持RESTful访问的服务标准.

#### WMTS
(OpenGIS Web Map Title Service)WMTS提供了一种采用预定义图块方法发布数字地图服务的标准化解决方案。WMTS弥补了WMS不能提供分块地图的不足。WMS针对提供可定制地图的服务，是一个动态数据或用户定制地图（需结合SLD标准）的理想解决办法。WMTS牺牲了提供定制地图的灵活性，代之以通过提供静态数据（基础地图）来增强伸缩性，这些静态数据的范围框和比例尺被限定在各个图块内。这些固定的图块集使得对WMTS服务的实现可以使用一个仅简单返回已有文件的Web服务器即可，同时使得可以利用一些标准的诸如分布式缓存的网络机制实现伸缩性

WMTS接口支持的三类资源：

+ 一个服务元数据（ServiceMetadata）资源（面向过程架构风格下对GetCapabilities操作的响应）（服务器方必须实现）。ServiceMetadata资源描述指定服务器实现的能力和包含的信息。在面向过程的架构风格中该操作也支持客户端与服务器间的标准版本协商。
+ 图块资源（对面向过程架构风格下GetTile操作的响应）（服务器方必须实现）。图块资源表示一个图层的地图表达结果的一小块。
+ 要素信息（FeatureInfo）资源（对面向过程架构风格下GetFeatureInfo操作的响应）（服务器方可选择实现）。该资源提供了图块地图中某一特定像素位置处地物要素的信息，与WMS中GetFeatureInfo操作的行为相似，以文本形式通过提供比如专题属性名称及其取值的方式返回相关信息

#### WFS
网络要素服务（WFS）支持用户在分布式的环境下通过HTTP对地理要素进行插入，更新，删除，检索和发现服务。该服务根据HTTP客户请求返回要素级的GML(Geography Markup Language、地理标识语言)数据，并提供对要素的增加、修改、删除等事务操作，是对Web地图服务的进一步深入。WFS通过OGC Filter构造查询条件，支持基于空间几何关系的查询，基于属性域的查询，当然还包括基于空间关系和属性域的共同查询。

WFS提供如下操作:
+ GetCapabitities：返回服务级元数据，它是对服务信息内容和要求参数的一种描述。
+ DescribeFeatureType：生成一个Schema用于描述WFS实现所能提供服务的要素类型。Schema描述定义了在输入时WFS实现如何对要素实例进行编码以及输出时如何生成一个要素实例。
+ GetFeature：可根据查询要求返回一个符合GML规范的数据文档。
+ LockFeature：用户通过Transaction请求时，为了保证要素信息的一致性，即当一个事务访问一个数据项时，其他的事务不能修改这个数据项，对要素数据加要素锁。
+ Transaction： 与要素实例的交互操作。该操作不仅能提供要素读取，同时支持要素在线编辑和事务处理。Transaction操作是可选的，服务器根据数据性质选择是否支持该操作。

#### WCS
网络覆盖服务是面向空间影像数据，它将包含地理位置的地理空间数据作为"覆盖（Coverage）"在网上相互交换，如卫星影像、数字高程数据等栅格数据。

WCS提供如下操作:
+ GetCapabitities：返回服务级元数据，它是对服务信息内容和要求参数的一种描述。
+ DescribeCoverage：支持用户从特定WCS服务器获取一个或多个覆盖的详细的描述文档。
+ GetCoverage：可根据查询要求返回一个包含或者引用被请求的覆盖数据的响应文档。

#### WPS
Web Processing Server（WPS）是新近推出的标准，它的功能其实我们已经耳熟能详了。Processing即ArcView中的GeoProcessing，诸如Union，Intersect等方法。WPS要做的就是暴露基于URL接口来实现客户端通过WebService对此类方法的调用、并返回数据。

#### 总结
+ WMS：动态地图服务，在ArcGIS中我们经常利用理由的mxd文件发布的服务，就是这种地图服务，如果你的数据会变化，建议发这种服务。这种服务优点是动态，缺点是慢。
+ WMS-C：可以理解为WMS的升级版，预先缓存瓦片，按需请求，提高了访问的速度。
+ WMTS：相比WMS，牺牲了提供定制地图的灵活性，代之以通过提供静态数据（基础地图）来增强伸缩性，这些静态数据的范围框和比例尺被限定在各个图块内。
+ WFS：支持要素的增删改等事务操作，支持空间和属性查询。
+ WCS：我理解的是WCS主要是面向空间影像数据
+ WPS：这块我理解的主要是用来发起web端的空间运算处理工作，入裁切、合并等空间运算。


## 二.几种常用的地图框架

### Leaflet
` 特点： 开源免费，适用于移动端，有很多第三方插件

` 缺点: 功能比较少，很多需要自己实现， 如图层的显示隐藏，并且图层层级问题不是很好解决

` 优点: 如infowindow支持 html形式注册，注册方便, 在地图使用后台返回json数据绘制责任网格上的文字比较复杂，需要使用canvas

### Arcgis
` 特点: 商用但免费，适用初学者，api丰富，功能丰富，第三方插件并不多

` 缺点: 需要提前在服务器上部署,如果加载天地图，地图加载较慢(不知道是不是偶现),同时，依赖于dojo，项目体积较大

` 优点: 功能齐全，控制图层方便,如featureLayer 等， 另外有一个专门放置marker的图层 Graphics 图层， 在Arcgis中marker叫做 Graphic,十分方便控制。 绘制责任网格上的文字较为方便，且无图层层级问题

[详细文档]: file:///Users/jackshen/Desktop/宜宾项目代码/Gis相关/GIS资料.pdf



### 百度地图，谷歌地图，高德地图等
` 特点: 商用，部分免费，部分收费。

` 缺点: 各家Api和实现方式不同。需要熟悉自家api

### mapbox. openlayer等


## 三.开发地图遇见的一些困难及解决方案

### 1. 需要绘制大量marker,会造成性能问题

~ 解决方案:

1. 使用聚合
2. 使用WMS，地图瓦片服务，gis组由GEOserver提供服务
3. 使用canvas绘制marker
4. 使用webgl绘制marker

#### 几种地图具体的解决方案

` 1.leaflet
使用leaflet-canvas-marker 进行绘制 marker,但是此插件不完美
1. 如果将所有marker清除后，地图异常(已解决)
2. 如果清除，再度绘制会导致图层层级不对，使用groupLayer继承的 bringtoback(将canvas绘制的东西移动到图层最后) 以及 setIndex 都不管用
3. 清除时必须要使用onRemove 传入map对象
4. 经纬度重叠的marker不会覆盖，会显示一部分阴影

` 2.arcgis
目前没找到能解决的插件,但是其本身优化比较好,1W+ 左右的marker能够支持

` 3.百度地图

1. 使用mapV通过GEOJSON绘制(缺点，鼠标移入的时候不会变成指针)

## 四.GIS团队提供的支持


### web-gis前端组件库 - 前端

一些功能的示例

[详细文档]: file:///Users/jackshen/Desktop/公司-GIS接口文档/【PRD】统一研发平台V2.0_统一GIS平台_V1.0_20190624.docx

| 功能点                                      | 具体出处                                                     | 备注                                               |
| ------------------------------------------- | ------------------------------------------------------------ | -------------------------------------------------- |
| 百度地图加载wms图层，如街道，社区，责任网格 | 宜宾综合指挥上由百度地图提供转化接口(收费)，然后加载到地图上 | 宜宾线上版本目前全部转化为百度坐标的json文件来实现 |
| 百度地图自定义绘制                          | 提供该功能，未应用                                           |                                                    |
| 天地图查询地名                              | 宜宾环卫系统查询具体地名                                     |                                                    |
| leaflet点聚合功能                           | -                                                            | gis-web组件提供功能                                |
| leaflet热力图功能                           | -                                                            | gis-web组件提供功能                                |


#### 1.加载wms图层代码示例
- 具体位置: git仓库: https://git.citycloud.com.cn:3000/gis/gis-web.git 分支: baidu

代码示例
```javascript
/**
 * 添加wms图层
 * @param {string} URL
 * @param {string} layers
 * @param {boolean} add 是否添加到地图中
 */
function Wms(map, URL, layers, add = true) {
  const BMap = window.BMap;
  const tileLayer = new BMap.TileLayer({ isTransparentPng: true });
  // 百度坐标转换工具
  const BaiduPointConvert = function(map) {
    this.map = map;
    //瓦片xy计算出经纬度坐标
    //step1: this.tileToPixel(pixel);百度地图瓦片大小为 256*256，根据 瓦片xy * 256计算出瓦片的像素坐标；
    //step2 : this.pixelToWorld(this.tileToPixel(pixel)) ; 将像素坐标转为平面坐标。
    //step3: this.worldToLngLat(this.pixelToWorld(this.tileToPixel(pixel))); 调用API提供的方法将平面坐标转为经纬度坐标；
    //API说明请参考：http://developer.baidu.com/map/reference/index.php?title=Class:%E5%9C%B0%E5%9B%BE%E7%B1%BB%E5%9E%8B%E7%B1%BB/Projection
    this.tileToLngLat = function(pixel) {
      return this.worldToLngLat(this.pixelToWorld(this.tileToPixel(pixel)));
    };
    this.lngLatToTile = function(lngLat) {
      return this.pixelToTile(this.worldToPixel(this.lngLatToWorld(lngLat)));
    };
    this.pixelToLngLat = function(pixel) {
      return this.worldToLngLat(this.pixelToWorld(pixel));
    };
    this.lngLatToPixel = function(lngLat) {
      return this.worldToPixel(this.lngLatToWorld(lngLat));
    };
    this.tileToPixel = function(pixel) {
      return new BMap.Pixel(pixel.x * 256, pixel.y * 256);
    };
    this.pixelToWorld = function(pixel) {
      var zoom = this.map.getZoom();
      return new BMap.Pixel(
        pixel.x / Math.pow(2, zoom - 18),
        pixel.y / Math.pow(2, zoom - 18)
      );
    };
    this.worldToLngLat = function(pixel) {
      var projection = this.map.getMapType().getProjection();
      return projection.pointToLngLat(pixel);
    };
    this.pixelToTile = function(pixel) {
      return new BMap.Pixel(pixel.x / 256, pixel.y / 256);
    };
    this.worldToPixel = function(pixel) {
      var zoom = this.map.getZoom();
      return new BMap.Pixel(
        pixel.x * Math.pow(2, zoom - 18),
        pixel.y * Math.pow(2, zoom - 18)
      );
    };
    this.lngLatToWorld = function(lngLat) {
      var projection = this.map.getMapType().getProjection();
      return projection.lngLatToPoint(lngLat);
    };
  };
  tileLayer.getTilesUrl = tileCoord => {
    const x = tileCoord.x;
    const y = tileCoord.y;
    //通过baiduMap API提供的方法将平面坐标转成经纬度坐标
    const PointConvert = new BaiduPointConvert(map);
    const lonlat_0 = PointConvert.tileToLngLat(tileCoord); //瓦片左下角坐标
    const tileCoord2 = new Object();
    tileCoord2.x = x + 1;
    tileCoord2.y = y + 1;
    const lonlat2_0 = PointConvert.tileToLngLat(tileCoord2); //瓦片右上角坐标；
    const _lonlat_0 = bd09towgs84(lonlat_0.lng, lonlat_0.lat);
    const _lonlat2_0 = bd09towgs84(lonlat2_0.lng, lonlat2_0.lat);
    const bbox = [_lonlat_0[0], _lonlat_0[1], _lonlat2_0[0], _lonlat2_0[1]]; //左下角与右上角坐标构成一个bbox；
    const url = `${URL}?SERVICE=WMS&REQUEST=GetMap&LAYERS=${layers}&STYLES=&FORMAT=image/png&TRANSPARENT=true&VERSION=1.1.1&WIDTH=256&HEIGHT=256&SRS=EPSG:4326&BBOX=${bbox.join(
      ","
    )}`;
    return url;
  };
  if (add) {
    map.addTileLayer(tileLayer);
  }
  return tileLayer;
}
```

#### 2.百度地图自定义绘制示例

- 具体位置: git仓库: https://git.citycloud.com.cn:3000/gis/gis-web.git 分支: baidu
- 自定义绘制轨迹

![20200421171357](http://img.jackshen.top/markdownimg/20200421171357.png)

- 代码示例
```javascript
<template>
  <div class="setting-trace">
    <div>
      <BaseMap @init="init"></BaseMap>
    </div>
    <div>
      <el-button @click="startDraw()">开始绘制</el-button>
      <el-button @click="editFence()">修改</el-button>
    </div>
  </div>
</template>

<script>
import BaseMap from '@/components/BaseMap.vue'
export default {
  name: "SettingTrace",
  components: {
    BaseMap
  },
  data(){
    return {
      map:null,
      drawingManager: null,
      polyline: null
    }
  },
  methods: {
    init(map) {
      this.map = map
      const overlaycomplete = (e) => {
        this.polyline = e.overlay
        console.log(e.overlay.getPath())
      };
      const styleOptions = {
        strokeColor: "red",    //边线颜色。
        fillColor: "red",      //填充颜色。当参数为空时，圆形将没有填充效果。
        strokeWeight: 3,       //边线的宽度，以像素为单位。
        strokeOpacity: 0.8,	   //边线透明度，取值范围0 - 1。
        fillOpacity: 0.6,      //填充的透明度，取值范围0 - 1。
        strokeStyle: 'solid' //边线的样式，solid或dashed。
      }
      //实例化鼠标绘制工具
      const drawingManager = new window.BMapLib.DrawingManager(map, {
        isOpen: false, //是否开启绘制模式
        enableDrawingTool: false, //是否显示工具栏
        circleOptions: styleOptions, //圆的样式
        polylineOptions: styleOptions, //线的样式
        polygonOptions: styleOptions, //多边形的样式
        rectangleOptions: styleOptions //矩形的样式
      });
      //添加鼠标绘制工具监听事件，用于获取绘制结果
      drawingManager.addEventListener('overlaycomplete', overlaycomplete);
      drawingManager.setDrawingMode(window.BMAP_DRAWING_POLYLINE)
      this.drawingManager = drawingManager
    } ,
    startDraw(){
       this.drawingManager.open()
    },
    editFence(){
      if(!this.polyline){
        return
      }
      this.polyline.enableEditing();
      this.map.addEventListener("dblclick",()=>{
        this.polyline.disableEditing();
         console.log(this.polyline.getPath())
      })
    }
  }
}
</script>
```

#### 3.天地图查询地名

+ 通过gis组提供的接口查询地名

- 具体位置: git仓库: https://git.citycloud.com.cn:3000/gis/gis-web.git 分支: 5.0.2

![20200422154906](http://img.jackshen.top/markdownimg/20200422154906.png)


[GIS后端接口]: file:///Users/jackshen/Desktop/公司-GIS接口文档/GIS后端接口文档_20191107.docx

#### 4.leaflet点聚合功能

- 具体位置: git仓库: https://git.citycloud.com.cn:3000/gis/gis-web.git 分支: 5.0.2

![20200422155448](http://img.jackshen.top/markdownimg/20200422155448.png)

#### 5.leaflet热力图功能

- 具体位置: git仓库: https://git.citycloud.com.cn:3000/gis/gis-web.git 分支: 5.0.2

![20200422155448](http://img.jackshen.top/markdownimg/20200422155448.png)

### GEOserver 后端服务

| 功能点                   | 具体出处                                   | 备注 |
| ------------------------ | ------------------------------------------ | ---- |
| 百度地图或天地图坐标转换 | 宜宾城管及综合指挥中录入坐标等操作         |      |
| 根据经纬度查询行政区划等 | 宜宾项目之前的工单登记功能需要- 后来去除了 |      |
|                          |                                            |      |

#### 1. 百度地图或天地图坐标转换

- gis后端提供的转换功能

- 具体地址 http://101.205.120.152:9098/position-web/swagger-ui.html#!/2235226631319953671625442/getConvertResultUsingPOST

![20200423151519](http://img.jackshen.top/markdownimg/20200423151519.png)

#### 2. 根据经纬度查询行政区划

![20200423151909](http://img.jackshen.top/markdownimg/20200423151909.png)

- 具体地址 详见GIS后端接口文档 7.4

## 其他解决方案

### argis 热力图

+ 成都城管原来的gis地图，使用热力图展示

- 具体位置: git仓库: ssh://git@git.citycloud.com.cn:3022/Project_cdcg/cdcg_screen_pre.git 分支: master

![20200422164201](http://img.jackshen.top/markdownimg/20200422164201.png)

### arcgis 点聚合

+ 成都城管原来的gis地图, 使用点聚合展示

- 具体位置: git仓库: ssh://git@git.citycloud.com.cn:3022/Project_cdcg/cdcg_screen_pre.git 分支: master

![20200422163927](http://img.jackshen.top/markdownimg/20200422163927.png)

### leaflet 绘制责任网格时添加文字，并且使用canvas

+ 成都城管新版gis地图，绘制行政区划等

+ 基于leaflet-canvas-marker + canvas 绘制文字

- 具体位置: git仓库: ssh://git@git.citycloud.com.cn:3022/Project_cdcg/cdcg_screen_pre.git 分支: screen_pre_v2

- 优点:不会遮盖marker，zIndex是统一的,性能较好 缺点:缩放的时候会重新绘制

- 代码示例
```javascript
  gegnerateWordPngUrl(canvas, wordArr) {
    let pngObj = {}
    canvas.width = 100;
    canvas.height = 14;
    var context = canvas.getContext("2d");
    //绘制背景画布
    context.fillStyle = "rgba(255,255,255,0)";
    context.font = "normal bolder 12px serif"; //设置字体
    context.fillStyle = "#000"; //字体颜色
    context.textAlign = "left"; //文本的中心被放置在指定的位置。
    context.textBaseline = "top"; //文本基线是 em 方框的正中。
    wordArr.forEach(item => {
      context.clearRect(0, 0, canvas.width, canvas.height)
      context.fillStyle = '#000'
      context.fillText(item, 0, 0)
      pngObj[item] = {
        url: canvas.toDataURL("image/png")
      }
    })
    return pngObj
  }
  drawDutyMapTitle(layers) {
    if (this.dutymapTitleLayer) {
      try {
        this.dutymapTitleLayer.onRemove(this.map)
        this.dutymapTitleLayer = null
      } catch(error) {
        console.warn(error,'-错误')
      }
    }
    let markers = []
    var nameArr = Object.keys(layers).map(key => {
      return layers[key].feature.properties.name
    })
    var pngObj = this.gegnerateWordPngUrl(this.wordcanvas, nameArr)
    Object.keys(layers).forEach(key => {
      const Bounds = layers[key].getBounds() //获取视野参数
      let centerLat = (Bounds._southWest.lat + Bounds._northEast.lat) / 2
      let centerLng = (Bounds._southWest.lng + Bounds._northEast.lng) / 2
      let name = layers[key].feature && layers[key].feature.properties.name
      if (pngObj[name]) {
        //绘制文字点位
        let myIcon = L.icon({
          iconUrl: pngObj[name].url,
          iconSize: [100, 14],
          iconAnchor: [50, 17]
        });
        let marker = L.marker([centerLat, centerLng], {
          icon: myIcon
        })
        markers.push(marker)
      }
    })
    this.dutymapTitleLayer = L.canvasIconLayer({}).addTo(this.map)
    this.dutymapTitleLayer.addLayers(markers)
  }
```

![20200422163045](http://img.jackshen.top/markdownimg/20200422163045.png)

### 高性能绘制marker

#### 后端优化

— 成都城管项目使用gzip压缩，优化大量数据处理的加载时间

![20200423165603](/Users/jackshen/Desktop/20200423165603.png)

#### leaflet 使用 leaflet-canvas-marker

- 支持10w+

- 具体位置: git仓库: ssh://git@git.citycloud.com.cn:3022/yb_code/yb_city_show_fore.git 分支: develop

- 插件位置 http://10.12.102.194:4873/-/web/detail/leaflet-canvas-marker

- 注意: 发布于内网npm，使用时需要连接内网

#### 百度地图或者谷歌地图或高德地图 使用mapv

- 支持6w+

- 具体位置: git仓库: ssh://git@git.citycloud.com.cn:3022/yb_code/yb_city_show_fore.git 分支: develop


### 高性能绘制polyline,polygon, 即线段和面

+ 因原始的marker绘制多采用html的img,div,span等元素呈现，这种方式的优点是生成的marker能够控制层级，方便进行图层控制, 缺点很明显，因为涉及到多个marker位置重新计算，所以在2000+左右的marker下就有点力不从心了

+ 当然,对于这种情况，有折中的解决方案

#### 1.点聚合方式

+ 在地图缩放比例较大的时候，计算该区域的所有覆盖物，位置接近的统一以数字进行展示

+ 能相对解决性能问题,不过对于数据在某个区域比较密集的情况，可能仍然存在性能问题

#### 2.类似arcgis底层实现的在视图内绘制，不在视图内，则放置入缓冲区中

+ 利用类似 self._map.getBounds().contains(latlng) 的api 判断绘制marker

#### leaflet 开启canvas render模式

- 分为全局开启，和部分开启，建议仅部分开启，全局开启会有异常

```javascript
1. 全局开启
    const map = L.map(mapid, {
      center,
      zoom,
      preferCanvas: true, // 全局开启canvas选项，leaflet绘制circle，绘制polyline均会采用canvas绘制
      doubleClickZoom: false,
      zoomControl: false, // 默认的放大缩小插件是英文的
      attributionControl: false // 是否显示leaflet广告
    })

2. 部分开启
   var myRenderer = L.canvas({
      padding: 0.5
    });
    layerGeo = L.geoJSON(myarr, {
      style: myStyle,
      renderer: myRenderer
    })
```

#### 百度地图或高德地图 使用mapv进行绘制

- mapv官网 `https://mapv.baidu.com/`

- 现有缺点， leaflet使用mapv绘制时，mouseover事件注册异常，故在考虑leaflet和百度，高德地图等兼容时，考量这一点

- 绘制icon时，需要先加载图片，建议使用base64图片，方便加载

- 示例如下

![20200423142708](http://img.jackshen.top/markdownimg/20200423142708.png)

#### supermap、 使用webgl技术进行绘制，由于收费，不深入探讨


### 高性能集中删除marker

- 普通删除marker的方式有几种
+ 1. leaflet中
+ 1.1 使用remove方法单独清除
+ 1.2 将marker集中为grouplayer，使用remove方法清除grouplayer

+ 2. 百度地图中
+ 2.1 使用removeOverlayer 方法清除

- 缺点很明显，在3000+ 左右的marker清除，重绘下，调用绘制UI界面宕机，十分影响体验，所以一方面我们需要使用canvas绘制marker,同时，canvas清除marker底层需要遍历marker一遍，如果每次重新绘制，都需要重新遍历，也会造成宕机

- 以下以leaflet-canvas-marker 和mapv的实践为例子

- 注意: 集中删除主要是利用有重绘api或者批量设置marker的能力，如果没有该能力，需要自己实现。因为有的地图架构每次清除marker都会执行重绘

#### leaflet-canvas-marker 集中删除marker

+ 宜宾综合指挥项目中， 车辆，人等使用canvas绘制， 而定位点使用原生绘制

![20200424152736](http://img.jackshen.top/markdownimg/20200424152736.png)

+ 批量删除时，区分原生绘制和canvas绘制，分别删除, 同时，leaflet-canvas-marker底层机制在removeMarker后，不会重新绘制,所以批量删除后，使用 `redraw` 方法来重绘

```javascript
  /**
   *
   * @param {*} arr 包含markerId的数组，集中清除后然后 redraw
   *
   */
  removeLayerGroup(arr) {
    if (!arr || !arr[0]) {
      console.warn('未获取到markerIds')
      return
    }
    const canvasGroup = []
    const defaultGroup = []
    arr.forEach(id => {
      const layer = this.featureCollectors[id]
      if (layer) {
        if (layer.options && layer.options.marker && layer.options.marker.isCanvas) {
          canvasGroup.push(layer)
        } else {
          defaultGroup.push(layer)
        }
      }
    })
    canvasGroup.forEach(canvasMarker => {
      try {
        if (this.canvasLayer) {
          this.canvasLayer.removeMarker(canvasMarker)
        }
      } catch (error) {
        console.log('移除失败')
      }
    })
    if (canvasGroup[0] && this.canvasLayer) {
      this.canvasLayer.redraw()
    }
    defaultGroup.forEach(defaultMarker => {
      defaultMarker.remove()
    })
    arr.forEach(type => {
      delete this.featureCollectors[type]
    })
  }
```

#### 百度地图mapv 集中删除marker

+ mapv提供的删除方案是一个filter函数，我们只需要将需要删除的数组传递进去就可以了,mapv提供的Dataset数据格式在set方法调用时，会集中重新绘制

```javascript
  /**
   *
   * @param {*} arr
   *
   */
  removeLayerGroup(arr) {
    if (!arr || !arr[0]) {
      console.warn('未获取到markerIds')
      return
    }
    const canvasGroup = []
    const defaultGroup = []
    arr.forEach(id => {
      const layer = this.markersCollectors[id]
      if (layer) {
        if (layer.isCanvas) {
          canvasGroup.push(layer)
        } else {
          defaultGroup.push(layer)
        }
      }
    })
    if (canvasGroup[0] && this.mapvDataset) {
      const _ids = canvasGroup.map(marker => {
        if (!marker.isLoaded && this.loadedImgObj[marker.iconUrl]) {
          const loadingArr = this.loadedImgObj[marker.iconUrl].loadingArr
          this.loadedImgObj[marker.iconUrl].loadingArr = loadingArr.filter(item => {
            return item.markerUId == marker.markerUId
          })
        }
        return marker.markerUId
      })
      const _data = this.mapvDataset.get({
        filter: function(item) {
          return !_ids.includes(item.marker.markerUId)
        }
      })
      this.mapvDataset.set(_data)
    }
    defaultGroup.forEach(defaultMarker => {
      this.map.removeOverlay(defaultMarker)
    })
    arr.forEach(type => {
      delete this.markersCollectors[type]
    })
  }
```
