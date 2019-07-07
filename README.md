# 清晰度源插件

---

videojs选择不同清晰度源插件（videojs6）

## 何时使用

- 切换不同清晰度的时候

## 浏览器支持

IE 9+

## 安装

```bash
npm install videojs-resolution-video --save
```

## 运行

```bash
# 默认开启服务器，地址为 ：http://local:8000/

# 能在ie9+下浏览本站，修改代码后自动重新构建，且能在ie10+运行热更新，页面会自动刷新
npm run start

# 构建生产环境静态文件，用于发布文档
npm run site
```

## 代码演示

### 基本

选择清晰度

```css
.main-container {
  overflow: visible;
}
.markdown ul > li {
  margin-left: 0;
  padding-left: 0;
}
```

```jsx
import videojs from 'video.js'
import 'video.js/dist/video-js.css'
import '@my-videojs/videojs-resolution-switcher'
import '@my-videojs/videojs-resolution-switcher/lib/style'

class App extends React.Component {
  componentDidMount () {
    const node = ReactDOM.findDOMNode(this.videoWrap)
    if (!node) {
      return
    }
    const videoJsOptions = {
      controls: true,
      plugins: {
        videoJsResolutionSwitcher: {
          default: 360,
          dynamicLabel: true,
          ui:true,
        }
      }
    }
    // react0.14.x data-reactid问题
    const videoEl = document.createElement('video')
    videoEl.className = `video-js`

    node.appendChild(videoEl)
    this.player = videojs(videoEl, {...videoJsOptions}, () => {
      this.addSwitcher(this.player)
    })
  }
  componentWillUnmount () {
    if (this.player) {
      this.player.dispose()
    }
  }
  addSwitcher (player) {
     player.updateSrc(
        [{
          src: '//www.w3school.com.cn/example/html5/mov_bbb.mp4',
          type: 'video/mp4',
          label: '标清',
          res: 360
        }, {
          src: '//clips.vorwaerts-gmbh.de/big_buck_bunny.mp4',
          type: 'video/mp4',
          label: '高清',
          res: 720
        }]
    );
    player.on('resolutionchange', function() {
        console.info('Source changed to %s', player.src())
    })
  }
  render() {
    return (
       <div data-vjs-player ref={node => { this.videoWrap = node }} />
    )
  }
}

ReactDOM.render(<App />, mountNode);
```

## API

代码是fork了[videojs-resolution-switcher](https://github.com/kmoskwiak/videojs-resolution-switcher)而来，原代码只支持videojs5.x,
所以修改了部分代码使其支持videojs6.x。API部分没有变化，可以参考[这里](https://kmoskwiak.github.io/videojs-resolution-switcher/)。
