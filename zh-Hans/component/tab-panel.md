# TabPanel  可横向滑动的面板

## 安装

```bash
$ npm install rax-tab-panel --save
```

## 引用

```jsx
import { TabController, TabPanel, TabPanelView, TabPanelLink } from 'rax-tab-panel';
```


## API说明


### 属性
|名称|类型|默认值|描述|
|:---------------|:--------|:----|:----------|
|isPanEnabled|Boolean|true|是否可以pan来横向滑动|
|isSlideEnabled| Boolean | true|是否可以有滑动slide效果|
|duration|Number|250|切换的动画周期，单位ms(仅在`useSlider:false`有效)|
|easing|String|cubic-bezier(0.25, 0.46, 0.45, 0.94)|动画缓动函数(仅在`useSlider:false`有效)|
|panDist|Number|375|判断滑动方向的阈值(仅在`useSlider:false`有效)|
|screenNumbersPerSide|Number|undefined|定义当前tabPanel两侧需要保留的panel数量，默认全部保留|
|extraBindingProps|Array|[]|根据tabPanel滑动所需进行的额外的binding效果 如：` [{element: this.refs.wrap,property: 'transform.translateX',expression:'x+0'}]` |
|beforeSwitch|Function|noop|切换到某个tab之前|
|afterSwitch | Function | noop |切换到某个tab之后|
|onViewAppear| Function | noop | 页面可见时触发(透传document的onViewAppear)|
|onViewDisAppear| Function | noop | 页面不可见时触发(透传document的onViewDisAppear)|
|useSlider|Boolean|false|是否用weex的`<slider>`组件来渲染|
|forbidSwipeBackOnIOS|Boolean,String|'auto'|是否阻止IOS上默认的侧滑返回功能，默认'auto'会在第0个panel时解除阻止|
|defaultFocusIndex|Number|0|默认聚焦到第几个panel|
|bounce|Boolean|true|滑动到边缘是否有会弹效果(useSlider下无效)|


### 方法

|名称|参数|返回值|描述|
|:---------------|:--------|:----|:----------|
|switchTo|(index, options)|{void}|切换到某个tab|

### 参数详解

#### index {Number} 索引

#### options

##### duration {Number}

## 基本示例

```jsx
/** @jsx createElement */
import {createElement, Component, render, findDOMNode} from 'rax';
import View from 'rax-view';
import Text from 'rax-text';
import {TabController, TabPanel, TabPanelView, TabPanelLink} from 'rax-tab-panel';
import transition from 'universal-transition';
import ScrollView from 'rax-scrollview';
import ListView from 'rax-listview';
import {isWeex} from 'universal-env';


const FULL_WIDTH = 750;

const DURATION = 250;

const styles = {
  tabBar: {
    top: 100
  },
  page: {},
  pageTxt: {
    fontSize: 60,
    lineHeight: 200,
    textAlign: 'center'
  }
};
let listData = [
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
  {name1: 'tom'}, {name1: 'tom'}, {name1: 'tom'},
];


class SamplePage extends Component {

  state = {
    data: [],
    index: 0
  }

  componentDidMount() {
    this.fetchData();
  }

  fetchData() {
    setTimeout(() => {
      this.state.index++;
      if (this.state.index < 5) {
        let data = this.state.data.concat(listData);
        this.setState({data});
      }
    }, 1000);
  }

  handleLoadMore = () => {
    this.fetchData();
  }

  renderRow = (item, index) => {
    return (<TabPanelView key={index} style={{height: 100, width: 750}}>
      <TabPanelLink style={{width: 750}} href="/demo">{item.name1}{index}</TabPanelLink>
    </TabPanelView>);
  }

  renderHeader = () => {
    return (<View style={{height: 100, width: 750}}>
      <Text>Page {this.props.index}</Text>
    </View>);
  }

  render() {


    return ( <View style={{flex: 1}}>
      {this.state.data.length > 0 ?
        <ListView renderHeader={this.renderHeader}
          renderRow={this.renderRow}
          dataSource={this.state.data}
          onEndReached={this.handleLoadMore} />
        : <View><Text>加载中...</Text></View>}
    </View>);
  }
}


const tabStyles = {
  container: {
    height: 100,
  },
  scrollContent: {
    flexDirection: 'row'
  },
  item: {
    width: 187.5,
    height: 100
  },
  itemTxt: {
    fontSize: 50,
    lineHeight: 100,
    textAlign: 'center'
  },
  block: {
    height: 100,
    position: 'absolute',
    left: 0,
    top: 0,
    backgroundColor: 'red'
  }
};


const tabItemWidth = [200, 250, 400, 250, 250];

const itemData = [{
  name: 'tab1',
  href: 'https://alibaba.github.io/rax/'
}, {
  name: 'tab2',
  href: 'https://alibaba.github.io/rax/'
},
{
  name: 'tab3',
  href: 'https://alibaba.github.io/rax/'
},
{
  name: 'tab4',
  href: 'https://alibaba.github.io/rax/'
},
{
  name: 'tab5',
  href: 'https://alibaba.github.io/rax/'
}];


function getLeft(widths, index) {
  let left = 0;
  for (let i = 0; i < index; i++) {
    left += widths[i];
  }
  return left;
}

class Tab extends Component {

  componentDidMount() {
  }

  switchTo(index, options = {duration: DURATION}) {
    let {type, duration} = options;
    let {
      beforeSwitch = () => {
      }, afterSwitch = () => {
      }
    } = this.props;
    let block = findDOMNode(this.refs.block);
    let left = getLeft(tabItemWidth, index);
    let itemWidth = tabItemWidth[index];

    beforeSwitch({
      index,
      type
    });

    // 移动Block
    transition(block, {
      transform: `translateX(${left}rem)`,
      width: `${itemWidth}rem`
    }, {
      timingFunction: 'ease-out',
      delay: 0,
      duration
    }, () => {
      afterSwitch({
        index,
        type
      });
    });
    let offset = left - FULL_WIDTH / 2 + itemWidth / 2 < 0 ? 0 : left - FULL_WIDTH / 2 + itemWidth / 2;
    this.refs.scrollView.scrollTo({x: offset});
  }


  render() {

    let {itemWidths, itemData} = this.props;

    return (<ScrollView {...this.props} style={this.props.style} horizontal={true} ref="scrollView">
      <View style={tabStyles.scrollContent}>
        <View style={[tabStyles.block, {width: itemWidths[0]}]} ref="block" />
        {itemData.map((item, i) => {
          return (
            <View style={[tabStyles.item, {width: itemWidths[i]}]} onClick={() => this.switchTo(i, {type: 'click'})}>
              <Text style={tabStyles.itemTxt}>{item.name}</Text>
            </View>);
        })}
      </View>
    </ScrollView>);
  }
}


class App extends Component {

  state = {}

  componentWillMount() {

  }

  componentDidMount() {
  }

  getTabBlockRef = () => {
    return this.refs.tab.refs.block;
  }

  beforeTabBarSwitch = (e) => {
    this.refs.tab.switchTo(e.index);
  }

  afterTabBarSwitch = (e) => {

  }

  beforeTabSwitch = (e) => {
    if (e.type === 'click') {
      this.refs.tabBar.switchTo(e.index);
    }
  }

  render() {

    return (
      <View style={{width: 750, position: 'absolute', top: 0, bottom: 0}}>
        <Tab itemWidths={tabItemWidth} itemData={itemData} style={tabStyles.container} ref="tab"
          beforeSwitch={this.beforeTabSwitch} />
        <TabController isPanEnabled={true} isSlideEnabled={true} style={styles.tabBar} ref="tabBar"
          beforeSwitch={this.beforeTabBarSwitch}
          afterSwitch={this.afterTabBarSwitch} tabBlockRef={this.getTabBlockRef}
          tabItemWidth={tabItemWidth}>
          <TabPanel style={styles.page}>
            <SamplePage index="0" />
          </TabPanel>
          <TabPanel style={styles.page}>
            <SamplePage index="1" />
          </TabPanel>
          <TabPanel style={styles.page}>
            <SamplePage index="2" />
          </TabPanel>
          <TabPanel style={styles.page}>
            <SamplePage index="3" />
          </TabPanel>
          <TabPanel style={styles.page}>
            <SamplePage index="4" />
          </TabPanel>
        </TabController>
      </View>
    );
  }


}


render(<App />);


```



## DEMO

#### 常规使用

- [简单demo](https://jsplayground.taobao.org/raxplayground/f132cf66-330f-482e-a998-1d8fa8692401)
- [结合tab-header组件使用](https://jsplayground.taobao.org/raxplayground/910d57b3-b6cb-4194-80d4-8dd2837fd14f)
- [配合导航做动画](https://jsplayground.taobao.org/raxplayground/f8e2196d-ec19-490f-86a8-b5f9bcc92eca)
- [嵌套](https://jsplayground.taobao.org/raxplayground/ff98f94a-1287-4da2-a5fd-3a911131d78f)






