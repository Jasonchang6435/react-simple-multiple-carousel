# react-simple-multiple-carousel
pure react easy impletation for multiple carousel

### DEMO
[DEMO](https://ewjtx.csb.app/)

###源码
```
import React from "react";
import ReactDOM from "react-dom";

import "./styles.css";

class App extends React.Component {
  constructor(props) {
    super();
    this.state = {
      imgList: [
        "https://ss3.baidu.com/-fo3dSag_xI4khGko9WTAnF6hhy/image/h%3D300/sign=b5e4c905865494ee982209191df4e0e1/c2cec3fdfc03924590b2a9b58d94a4c27d1e2500.jpg",
        "https://ss0.baidu.com/94o3dSag_xI4khGko9WTAnF6hhy/image/h%3D300/sign=a62e824376d98d1069d40a31113eb807/838ba61ea8d3fd1fc9c7b6853a4e251f94ca5f46.jpg",
        "https://ss1.baidu.com/9vo3dSag_xI4khGko9WTAnF6hhy/image/h%3D300/sign=92afee66fd36afc3110c39658318eb85/908fa0ec08fa513db777cf78376d55fbb3fbd9b3.jpg",
        "https://ss2.baidu.com/-vo3dSag_xI4khGko9WTAnF6hhy/image/h%3D300/sign=0c78105b888ba61ec0eece2f713597cc/0e2442a7d933c8956c0e8eeadb1373f08202002a.jpg"
      ],
      interval: 0,
      //  next++ prev--
      activeFirsrtIndex: 0,
      activeIndexs: [],
      translateIndexList: [],
      showItems: 3,
      step: 1
    };
    this.updateTranslates = this.updateTranslates.bind(this);
    this.next = this.next.bind(this);
    this.prev = this.prev.bind(this);
    this.getTranslateX = this.getTranslateX.bind(this);
  }

  next() {
    this.updateTranslates(-this.state.step);
  }

  prev() {
    this.updateTranslates(this.state.step);
  }

  getClassName(index) {
    return "div-item";
  }

  getTranslateX(index) {
    let self = this;
    let { translateIndexList, showItems } = self.state;
    let x = translateIndexList[index] * 390 + "px";
    let v = translateIndexList[index];
    if (v < 0 || v > showItems - 1) {
      return {
        left: x,
        opacity: 0
      };
    } else {
      return {
        left: x,
        transition: "all ease-in-out 0.4s"
      };
    }
  }

  initCarousel() {
    let self = this;
    let {
      imgList,
      activeFirsrtIndex,
      activeIndexs,
      translateIndexList
    } = self.state;
    let len = imgList.length;
    imgList = imgList.concat(imgList, imgList);
    for (let i = -len; i < len + len; i++) {
      translateIndexList.push(i);
    }
    //
    self.setState({
      imgList,
      activeFirsrtIndex,
      activeIndexs,
      translateIndexList
    });
  }

  updateTranslates(step) {
    let self = this;
    let { imgList, translateIndexList } = self.state;
    // 去掉增加的一张 4
    let len = imgList.length / 3;
    //
    translateIndexList = translateIndexList.map(index => {
      let i = index + step;
      //  -4
      if (i < -len) {
        // 7
        return len + len - 1;
        // 7
      } else if (i > len + len - 1) {
        // -4
        return -len;
      } else {
        return i;
      }
    });
    console.log("debug translateIndexList sadsada", translateIndexList);
    self.setState({ translateIndexList });
  }

  componentDidMount() {
    let self = this;
    self.initCarousel();
    let interval = setInterval(() => {
      self.next();
    }, 3000);
    this.setState({ interval });
  }

  componentWillUnmount() {
    clearInterval(this.state.interval);
  }

  render() {
    let { imgList, activeIndexs, translateIndexList } = this.state;
    return (
      <div className="App">
        <h1>Hello React-pure-simple-multiple-carousel</h1>
        <p>{activeIndexs.join(";")}</p>
        <p>{translateIndexList.join(";")}</p>
        <p>{imgList.length}</p>
        <button onClick={this.next}>NEXT</button>
        <button onClick={this.prev}>prev</button>
        <div className="carousel">
          {imgList.map((img, i) => (
            <div
              className={this.getClassName(i)}
              style={this.getTranslateX(i)}
              key={i}
            >
              <img className="img-item" alt={i} src={img} />
            </div>
          ))}
        </div>
      </div>
    );
  }
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);

```

### 样式
```
.App {
  font-family: sans-serif;
  text-align: center;
}
.carousel {
  position: relative;
  box-sizing: border-box;
  width: 1140px;
  height: 250px;
  overflow: hidden;
  /* border: 1px solid red; */
}
.div-item {
  position: absolute;
  top: 0;
  box-sizing: border-box;
  width: 360px;
  height: 100%;
  margin-right: 30px;
  transition: all ease-in-out 0.4s;
  /* border: 1px dashed blue; */
}

.img-item {
  width: 100%;
  height: 100%;
}


```
