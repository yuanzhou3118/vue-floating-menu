# Vue-Floating-Menu

[![Netlify Status](https://api.netlify.com/api/v1/badges/333eb4f0-852a-40ad-a5d0-bad45d4ef3bc/deploy-status)](https://vue-floating-menu.netlify.app)

## 前言

**正如这个名字，这是一个具有拖拽吸附功能的浮窗菜单，开源项目**

> 一个基于 vue 的浮窗组件,可在屏幕内自由拖拽，拖拽后可以根据最后的位置吸附到页面两边，而且可以点击浮窗显示菜单

**附上 github 项目地址** [vue-floating-menu](https://github.com/yuanzhou3118/vue-floating-menu)

**效果如下:**
![vue-floating-menu](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bba487b021194edaa368f97cd0a6ccec~tplv-k3u1fbpfcp-zoom-1.image)

## 遇到的问题总结

- **鼠标移动过快，导致拖拽失焦：**

```html
<div @mousedown.stop.prevent="moveStart" @click.stop.prevent="toggleMenu"></div>
```

```js
moveStart(e) {
  // ... ...省略号... ...
  // 具体可以在github项目里查看

  document.onmousemove = async (e) => {
    this.clickFlag = false;
    this.moveFlags = true;
    // 💥👉在这里边处理拖拽时的位置更新,就是因为这个。
    // 我之前是单独通过监听mousemove的方法，所以遇到了这个问题
  };
  document.onmouseup = () => {
    document.onmousemove = null;
    document.onmouseup = null;
    this.moveEnd();
  };
},
```

- **判断是否是点击事件：**

```html
<div @mousedown.stop.prevent="moveStart" @click.stop.prevent="toggleMenu"></div>
```

```javascript
toggleMenu() {
  //  如果上一次down事件到下一次click事件中 相同即为点击事件
  if (this.lastMoveIndex == this.curMoveIndex) {
	//💥点击事件
  }
  this.curMoveIndex = this.lastMoveIndex;
},
moveStart(e) {
  // ... ...省略号... ...
  // 具体可以在github项目里查看
  document.onmousemove = async (e) => {
    this.lastMoveIndex++;
  };
  document.onmouseup = () => {
    document.onmousemove = null;
    document.onmouseup = null;
    this.moveEnd();
  };
},
```

## Usage

Demo see here: https://vue-floating-menu.netlify.app

- **添加菜单的方式：**  
  组件 src/components/menuPart.vue 是添加菜单的 demo

**如果[vue-floating-menu](https://github.com/yuanzhou3118/vue-floating-menu)觉得不错的，记得给个 ⭐ 哟**

[![Edit Vue Floating Menu Examples](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/github/yuanzhou3118/vue-floating-menu)
