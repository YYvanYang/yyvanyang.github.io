# AI XXX 项目总结 总结

1. 百度地图加载问题(解决脚本加载阻塞问题)

优化前：

```html
<script type="text/javascript" src="//api.map.baidu.com/api?v=2.0&ak=R8eGyfVZXXXXXXXXXX"></script>
```

主要问题(会有脚本加载阻塞警告)：

>A parser-blocking, cross site (i.e. different eTLD+1) script, http://api.map.baidu.com/getscript?v=2.0&ak=R8eGyfVZXXXXXXXXXX&services=&t=20180323171755, is invoked via document.write. The network request for this script MAY be blocked by the browser in this or a future page load due to poor network connectivity. If blocked in this page load, it will be confirmed in a subsequent console message. See https://www.chromestatus.com/feature/5718547946799104 for more details.

优化后(用js异步加载baidu map js):

```JS
// loadMap.js
// load baidu map js
export function getMapScript () {
  if (!window.BMap) {
    const ak = "sdflsidfsfXXXXXXxxxxxxxxxxx"
    window.BMap = {}
    window.BMap._preloader = new Promise((resolve, reject) => {
      let $script = document.createElement( "script" )
      let head = document.head || document.documentElement;
      window._initBaiduMap = function () {
        resolve(window.BMap)

        // Remove the script
        if ( $script.parentNode ) {
          $script.parentNode.removeChild( $script );
        }

        // Dereference the script
        $script = null;

        window._initBaiduMap = null
        initBMapLib()
      }

      $script.src = `https://api.map.baidu.com/api?v=2.0&ak=${ak}&callback=_initBaiduMap`
      head.insertBefore( $script, head.firstChild );
    })
    return window.BMap._preloader
  } else if (!window.BMap._preloader) {
    return Promise.resolve(window.BMap)
  } else {
    return window.BMap._preloader
  }
}

```

```js
// view
// init baidu map
import { getMapScript } from "./loadMap.js";

getMapScript().then((BMap) => {
      window.BMap = window.BMap || BMap

      this.$nextTick(() => {
        this.initMap()
      })

    })

```


2. 弹窗及滑动效果实现(内部由panBy接口实现移动)

主要使用百度提供的类库（http://api.map.baidu.com/library/InfoBox/1.2/docs/symbols/BMapLib.InfoBox.html）

弹窗抖动效果实现
     ```css
     /* x轴翻转 */
      .show-popup-window {
        transition-property: all;
            transform-origin: center bottom;
            animation: flipInX .3s 1;
            backface-visibility: visible;
        }

        @keyframes flipInX {
            0% {
                transform: perspective(400px) rotateX(30deg);
                opacity: 1
            }
            40% {
                transform: perspective(400px) rotateX(-5deg)
            }
            70% {
                transform: perspective(400px) rotateX(5deg)
            }
            100% {
                transform: perspective(400px) rotateX(0deg);
                opacity: 1
            }
        }


    ```

    ```css
    /* y轴翻转 */
    .show-popup-window {
        transition-property: all;
        transform-origin: right center;
        animation: flipInY .3s 1;
        backface-visibility: visible;
      }

      @keyframes flipInY {
        0% {
          transform: perspective(400px) rotateY(10deg);
          opacity: 1
        }
        40% {
          transform: perspective(400px) rotateY(-3deg)
        }
        70% {
          transform: perspective(400px) rotateY(3deg)
        }
        100% {
          transform: perspective(400px) rotateY(0deg);
          opacity: 1
        }
      }
    ```

3. wuex的使用

因项目比较小，起初并没有使用vuex。但随着开发进度的展开，组件的嵌套层级越来越多，组件之间数据的层层传递变得越来越不好维护，
故开始引入vuex以解决组件之间传数据的问题。

4. 后台Api合并请求的改进

开发前期，后端提供的api接口太过零散，一个业务接口要请求n次，和后端讨论和，后端改进接口，做相应的请求合并，提升性能。

5. echarts的图标实现(http://echarts.baidu.com/option.html#legend.data.icon)

可以通过 'path://' 将图标设置为任意的矢量路径。这种方式相比于使用图片的方式，不用担心因为缩放而产生锯齿或模糊，而且可以设置为任意颜色。路径图形会自适应调整为合适的大小。路径的格式参见 [SVG PathData](https://www.w3.org/TR/SVG/paths.html#PathData)。可以从 Adobe Illustrator 等工具编辑导出。

```js
legend: {
      data: legend.map(x => {
        let _event = eventType[x]
        _event = _event ? _event : x
        return {
          name: _event,
          icon: 'path://M16,0 L2,0 C0.9,0 0,0.9 0,2 L0,16 C0,17.1 0.9,18 2,18 L16,18 C17.1,18 18,17.1 18,16 L18,2 C18,0.9 17.1,0 16,0 L16,0 Z M7,14 L2,9.19230769 L3.4,7.84615385 L7,11.3076923 L14.6,4 L16,5.34615385 L7,14 L7,14 Z'
        }
      }),
      bottom: 'bottom'
    },
    // ...
    ```