# Navigation  应用导航

Navigation提供了构建App的能力，您可以在特殊的场景使用，例如要实现转场动画，同时在Web端也实现了对等的效果。

## 安装

```bash
$ npm install rax-navigation --save
```

## 引用

```jsx
import {StackNavigator,TabNavigator} from 'rax-navigation';
```


## API说明

### StackNavigator 堆栈式导航

堆栈式导航是应用常用的右侧弹出一个新的窗口的导航，该类型导航可以做前进(push)、回退(pop)动作

#### 方法

|名称|参数|返回值|描述|
|:---------------|:--------|:----|:----------|
| StackNavigator |(routerConfig, options)|{Component}|创建一个堆栈式导航器|

### 参数详解

#### routerConfig {Object} 路由配置

可以通过`key,value`的形式创建路由表，格式参考如下：

```
{
   Home: {
    screen: HomeScreen,
    path: '/'
  },
  ItemDetail: {
    screen: ItemDetailScreen,
    path: '/items/:itemId'
  },
  Settings: {
    screen: SettingsScreen,
    path: '/settings'
  }
}

```


#### 属性
|名称|类型|默认值|描述|
|:---------------|:--------|:----|:----------|
|screen|Component||路由对应的视图|
|getScreen|Promise||路由对应的视图,和screen功能一样，支持Promise，最终只要resolve返回Component类型即可|
| path | String ||路由匹配路径规则，通过`:itemId`这样形式最终会在params里拿到itemId|

#### options {Object} 导航器全局配置


#### 属性
|名称|类型|默认值|描述|
|:---------------|:--------|:----|:----------|
|navigationOptions|Object,Function||导航配置，当前当行器下所有screen都统一使用的默认配置(可被screen下的static属性`navigationOptions`覆盖) |
|headerMode|String|'float'|导航弹出收起的特效，有`float`,`screen`,`none`三种|
|onTransitionStart|Function|noop|导航弹出或者收起时动画开始时触发|
|onTransitionEnd|Function|noop|导航弹出或者收起时动画结束时触发|
|transitionConfig|Object||导航容器转场动画配置|
|cardStyle|Object||卡片样式|
|mode|String|'hash'|history模式，默认采用`HashHistory`,可选`browser`使用`BrowserHistory`|
|loadingComponent|Component||loading组件，若设置为`null`则不展示|
|loadingText| String |'loading...'|loading文本|
|loadingIconSource|String||loading图片url|
|loadingTextStyle| Object ||loading文本样式|
|loadingIconStyle| Object ||loading图标样式|
|loadingWrapStyle|Object||loading内容器样式|





##### navigationOptions 详解

|名称|类型|默认值|描述|
|:---------------|:--------|:----|:----------|
|title|Component,String ||页面标题|
|header|Component||整个header，需返回一个Component,若设置成`null`，则隐藏导航|
|headerBackTitle| Component,String|`'Back'`|返回文案|
|headerLeft| Component,String||header左侧部分，需返回一个Component|
|headerRight| Component,String|| header右侧部分，需返回一个Component|
|headerTintColor| String ||header背景颜色|
|headerTransparent| Boolean ||header透明，此时页面从顶部渲染|

##### transitionConfig 详解

|名称|类型|默认值|描述|
|:---------------|:--------|:----|:----------|
|mask|Boolean|true|是否需要遮罩|
|transitionSpec|Object||下一屏进入的过渡动画配置|
|prevTransitionSpec|Object||上一屏进入的过渡动画配置|

示例：

比如需要进行一个屏幕从下放进入，并且带有缩放效果，上一屏从上方离开并带有缩放效果，可以这么写

```
StackNavigator(routerConfig,{
   //prevState.routeName
	transitionConfig:({prevState,state}) => ({
      mask: true,
      transitionSpec: {  //描述下一屏进入的动画
        initialStyle: {
          transform: `translateY(${windowHeight}rem)`,
          opacity: 0
        },
        duration: 350,
        props: [
          {
            property: 'transform.translateY',
            inputRange: [0, 1],
            outputRange: [windowHeight, 0]
          },
          {
            property: 'transform.scale',
            inputRange: [0, 1],
            outputRange: [0.9, 1]
          },
          {
            property: 'opacity',
            inputRange: [0, 1],
            outputRange: [0, 1]
          }
        ]
      },
      prevTransitionSpec: {  //描述上一屏进入的动画
        initialStyle: {
          transform: 'translateY(0rem)'
        },
        duration: 350,
        props: [
          {
            property: 'transform.translateY',
            inputRange: [1, 0],
            outputRange: [0, -windowHeight / 10]
          },
          {
            property: 'transform.scale',
            inputRange: [1, 0],
            outputRange: [1, 0.9]
          }
        ]
      })
    }
});

```



### TabNavigator 卡片式导航

卡片式导航是应用常用来做横向tab切换的导航

#### 方法

|名称|参数|返回值|描述|
|:---------------|:--------|:----|:----------|
| TabNavigator |(routerConfig, options)|{Component}|创建一个卡片式导航器|

### 参数详解

#### routerConfig {Object} 路由配置

可以通过`key,value`的形式创建路由表，格式同StackNavigator

#### 属性
|名称|类型|默认值|描述|
|:---------------|:--------|:----|:----------|
|screen|Component||路由对应的视图|
|getScreen|Promise||路由对应的视图,和screen功能一样，支持Promise，最终只要resolve返回Component类型即可|
| path | String ||路由匹配路径规则，通过`:itemId`这样形式最终会在params里拿到itemId|

#### options {Object} 导航器全局配置


#### 属性
|名称|类型|默认值|描述|
|:---------------|:--------|:----|:----------|
|tabBarOptions|Object||tabbar配置|
|navigationOptions|Object,Function||导航配置，当前当行器下所有screen都统一使用的默认配置(可被screen下的static属性`navigationOptions`覆盖) |
|tabBarPosition|String|`'bottom'`|tabBar的位置，`'bottom'`或者`'top'`|
|onTransitionStart|Function||tab切换开始时触发|
|onTransitionEnd|Function||tab切换结束时触发|
|screenNumbersPerSide| Number| undefined |定义当前tabPanel两侧需要保留的panel数量，默认全部保留|


##### tabBarOptions 详解

|名称|类型|默认值|描述|
|:---------------|:--------|:----|:----------|
|activeTintColor|String|`#000`|tabBar高亮时文字颜色|
|activeBackgroundColor|String|`#fff`|tabBar高亮时背景色|
|inactiveTintColor|String|`#fff`|tabBar非高亮时文字颜色|
|inactiveBackgroundColor|String|`#000`|tabBar非高亮时背景色|
|style|Object||tabBar样式|
|tabStyle| Object ||单个tab项的样式|

##### navigationOptions 详解

|名称|类型|默认值|描述|
|:---------------|:--------|:----|:----------|
|headerTitle|Component,String ||若嵌套在stackNavigator内，则可以设置导航标题，伴随tab切换时切换|
|header|Component||整个header，需返回一个Component,若设置成`null`，则隐藏导航|
|headerBackTitle| Component,String|`'Back'`|返回文案|
|headerLeft| Component,String||header左侧部分，需返回一个Component|
|headerRight| Component,String|| header右侧部分，需返回一个Component|
|headerTintColor| String ||header背景颜色|

### Navigation 导航器

每个页面内部可以通过`this.props.navigation`获取全局导航器

#### 方法

|名称|参数|返回值|描述|
|:---------------|:----------|:----------|:----------|
|navigate|`routeName`,`params`|void|页面导航跳转，历史记录长度+1|
|replace|`routeName`,`params`|void|页面导航跳转,历史记录长度不变|
|goBack|无|void|页面回退|
|go|`n`|void|页面前进或者后退|
|addListener|`eventName`,`handler`|void|监听导航事件|
|dispatch|NavigationAction|void|分发导航事件|

当前支持的事件有`willFocus`,`willBlur`,`didFocus`,`didBlur`

示例

```

class MyHomeScreen extends Component {
  static navigationOptions = ({navigation}) => ({
    ...
  });

  componentWillMount() {
    let {navigation} = this.props;

     navigation.addListener('willFocus', (e) => {
       //当前屏幕即将进入 相当于viewAppear事件
     });

     navigation.addListener('willBlur', (e) => {
      //当前屏幕即将离开 相当于viewDisappear事件
    });

     navigation.addListener('didFocus', (e) => {
       //当前屏幕完全进入
     });


     navigation.addListener('didBlur', () => {
       //当前屏幕完全离开
    });

  }

  render(){
    ...
  }
}

```





### Link 超链接

在单页面场景下，rax-link已经无法使用了，需要用该组件替换

使用方式

```

import {Link} from 'rax-navigation';


...

 <Link href='/demo/stack2.html'>Click me to stack2 page</Link>


```

### 属性
|名称|类型|默认值|描述|
|:---------------|:--------|:----|:----------|
|href|String||链接地址|



## 基本示例

```jsx
import {render, createElement, Component} from 'rax';
import {StackNavigator} from 'rax-navigation';
import Button from 'rax-button';

class MainScreen extends Component {
  static navigationOptions = {
    title: 'Welcome'
  };
  render() {
    const { navigate } = this.props.navigation;
    return (
      <Button
        title="Go to Jane's profile"
        onPress={() => {
          navigate('Profile', { name: 'Jane' })
        }}
      />
    );
  }
}

class ProfileScreen extends Component {
  static navigationOptions = {
    title: ({state}) => state.params.name,
    header: false
  };
  render() {
    const { goBack } = this.props.navigation;
    return (
      <Button
        title="Go back"
        onPress={() => goBack()}
      />
    );
  }
}

const BasicApp = StackNavigator({
  Main: {screen: MainScreen,path:'/'},
  Profile: {screen: ProfileScreen,path:'/profile'},
});

render(<BasicApp />);

```



## DEMO

#### 常规使用

- [单个Tab导航](https://jsplayground.taobao.org/raxplayground/da7214e3-813d-4904-86bd-059dbf6a399e)
- [单个Stack导航](https://jsplayground.taobao.org/raxplayground/0fe6c47c-e731-48fa-af99-87d24243c0eb)
- [Stack导航嵌套Tab导航](https://jsplayground.taobao.org/raxplayground/2becd385-722e-419a-ab9d-cf20f47dda2b)
- [自定义转场动画1](https://jsplayground.taobao.org/raxplayground/ed3da362-96c0-4607-9b28-62feead115d8)
- [自定义转场动画2](https://jsplayground.taobao.org/raxplayground/8bedeacd-b2f6-43bd-827f-5738cdb86004)
- [透明导航](https://jsplayground.taobao.org/raxplayground/aaa0e400-9ab0-4e38-91a1-b3b0e494d8e3)

