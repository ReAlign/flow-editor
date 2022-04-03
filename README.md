# flow-editor api

## DEMO

![png](https://haitao.nos.netease.com/5c795665-4e6f-496c-9ff2-c3d21afe000e_2904_1198.png)

* [online-demo](https://codepen.io/realign/full/EGxyzZ)

## 功能说明

* 待补充

## Usage

```javascript
// umd
import { FlowEditor } from 'flow-editor.min';
// esm
import FlowEditor from 'flow-editor.esm.min';
// browser
<script type="text/javascript" src="flow-editor.min.js"></script>
```

## API

### 实例化

```javascript
const flowEditor = new FlowEditor();
```

### 创建

```javascript
flowEditor.create(conf);

conf = {
    el: '#app',         // dom 节点
    data,               // 流程数据，见数据说明
    options,            // 流程配置，见数据说明
}

el          | String   | ''         | inject 节点
data        | Object   | {}         | 流程数据
options     | Object   | auto       | 内置流程配置
```

### 更新

```javascript
flowEditor.update(data);

data        | Object   | {}         | 流程数据
```

### 设置默认选中

```javascript
flowEditor.setSelected(path);

path        | String   | ''         | 节点路径

* 注：节点路径，就是相对于根节点的路径，如：
    * `children[0]`
    * `children[1].children[0]`
    * `children[0].children[1].children[2]`
```

### 缩放

> 触发太频繁会出现重影（堆叠），最好是调用间隔控制一下

> 已知问题，待修复

```javascript
flowEditor.scale(ratio);

ratio       | Number   | 1         | 缩放比例
```

### 生成图片

```javascript
flowEditor.getImage(opts);

withBg      | boolean   | true      | 是否带背景
type        | string    | 'png'     | 图片类型
base64      | boolean   | true      | 是否base64

@return     | Promise   | then(img) | Promise
```

### 查找

```javascript
flowEditor.find(data, nodePath, defVal);

data        // 流程数据
nodePath    // 选中操作中返回的 targetPath
defVal      // 默认值

@return     data 中的引用数据，用来更新编辑
```

### 选中节点

```javascript
flowEditor.selectedFENodes      // array

{
    target,                     // 节点
    targetPath,                 // 节点路径
    targetAttach,               // 节点相关信息
}
```

### 数据说明

#### 流程数据

```javascript
data = {
    text: 'node',                   // 显示文本
    shape: 'round-rectangle',       // 节点的形状
    highlight: {                    // 高亮
        type: 'error',              // 高亮类型
    },
    customData: {                   // 用户自定义

    },
    children: []                    // 子节点
}
```

#### 配置数据

```javascript
// 样式配置
options: {
    styles: {
        // 画布整体样式
        canvas: {
            bg: '#fff',
            bgImg: 'img url', // 优先于 bg
        },
        // 图形尺寸（全局）
        size: {
            width: 120,
            height: 36
        },
        // 图形之间的间距
        gutter: {
            vertical: 100, // 图形垂直间距 默认：100
            horizontal: 60, // 图形水平间距 默认：60
        },
        /**
        * @name 形状配置
        * key: shape / shape__(具体填充)
        */
        shape: {
            // 圆
            circle: {},
            // 矩形
            rectangle: {
                bg: '#FFBB00',                      // 背景色
                bgImg: '',                          // 背景图（优先）
                rBorder: 2,                         // 圆角度数
                text: {                             // 文本相关
                    family: 'Arial',                // 字体
                    color: '#333',                  // 颜色
                    size: 12,                       // 字号
                    lineHeight: 18,                 // 行高
                    align: 'left',                  // 对齐(有其他注意点)
                    indent: 46,                     // 偏移
                    maxLen: 7,                      // 显示最大字数，超出...
                },
                highlight: {                        // 高亮样式（会与hover、active叠加）
                    error: {                        // 具体高亮类型
                        bg: 'red',
                        border: '1px solid red',
                    }
                }
            },
            // 圆角矩形
            'round-rectangle': {},
            // 圆角矩形
            'round-rectangle__1': {},
        },
        // 连线
        line: {
            color: '#B4B4B4',                       // 线色
            width: 1,                               // 线宽
            borderRadius: 6                         // 拐点
        },
    },
    eventBind: {                                    // 事件绑定开关
        click: false,
        contextmenu: false,
        mouse: false,
    },
    events: {                                       // 绑定事件
        click(e) {},
        contextmenu(e) {},
        mousemove(e) {},
        mouseover(e) {},
        mouseout(e) {},
        clickEmpty(e) {}, // 点击空白区域，只有 eventBind 不全为 false 时有效
    }
},
```

#### 抛出事件对象

```javascript
e = {
    type: 'click',          // 事件类型
    target: FENode(),       // 作用节点
    targetPath: '',         // 作用节点路径
    targetAttach: {         // 作用节点相关信息
        parentPath: '',     // 父节点路径
        index: Number,      // children 中的位置
    },
    event: MouseEvent(),    // 原生鼠标事件对象
    eventFrom: '',          // 事件触发来源: { 'simulation'->模拟, 'trigger'->手动触发 }
    shapePoint: {           // 当前作用 shape 区域
        topLeft: { x, y, },
        topRight: { },
        bottomLeft: { },
        bottomRight: { },
    },
}
```
