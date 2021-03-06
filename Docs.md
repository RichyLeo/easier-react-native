# easier-react-native库

## 目录结构
> easier-react-native
>
> > base/
> >
> > http/
> >
> > utils/
> >
> > widgets/
> >
> > index.js
> >

**目录说明**

- **base**

	公共基类包
- **http**

	http请求工具类包
- **utils**

	工具类包
- **widgets**

	自定义控件包
- **index.js**

	库资源导出类


##库资源导出类

**index.js** 导出easier-react-native中所有可使用组件

**应用：**

```
import {
    BaseComponent,
    Button,
    //...
} from 'easier-react-native'
```


## 公共基类

### BaseComponent

**定义：** 所有Component类的基类

**说明：** 继承BaseComponent后，页面默认拥有TitleBar，但不能重写render()方法，需要重写renderBody()方法，用于渲染body内容，也可以在页面渲染完成之后调用setBody()方法重新渲染。如果需要重写界面，只需重写render()方法即可。关于TitleBar，下文有详细介绍。

**新的生命周期：componentDidFocus** 在component获得焦点的时候执行，可将API请求放在此执行，防止component卡顿

**方法：**

- **setBackgroundImage(source)** 设置背景图片，参数为图片资源
- **getTitleBar()** 返回一个TitleBar对象
- **setTitleBar(props:object)** 设置TitleBar属性，详见TitleBar
- **titleBarConfig()** 子类可重写，返回TitleBar属性Object，效果等同setTitleBar(props)
- **setTitle(title:string)** 设置TitleBar title属性
- **renderBody()** 子类可重写，返回body渲染内容
- **getLoading()** 返回Loading对象
- **showLoading(text:string, pointerEvents:bool)** - show loading
- **dismissLoading()** - dismiss loading
- **setLoadingCover(cover:string)** 设置loading背景覆盖范围，默认不覆盖TitleBar，cover='full-screen'覆盖全屏
- **startComponent(name:string, props:object)** Component页面跳转，name为Component，使用之前，请确保Component在Manifest中已注册
- **finish()** 关闭当前页面
- **finishToBefore(name:string, props:object, index:number)** 回到指定页面，若指定的页面不存在，则新建指定页面，index为新建页面放入Component栈的下标，默认为当前页的上一页，props为新建页面参数


**应用：**

```
'use strict';

import {
	BaseComponent
} from 'easier-react-native'

class TestComponent extends BaseComponent {

   	constructor(props) {
        super(props);
	    this.title = 'test title';
	    this.backgroundImage = require('./images/test.png');
	    this.loadingCover = 'full-screen';
	    //or
	    this.setTitle('test title');
	    this.setBackgroundImage(require('./images/test.png'));
	    this.setLoadingCover('full-screen');

	    this.setTitleBar({
	        //...titlebar config
	    });
	}

	renderBody() {
		return (
			<Text>I'm a body</Text>
		);
	}

}

module.exports = TestComponent;
```



##工具类

###InitUtil

**定义：** 初始化项目入口Component

**应用：**

修改index.ios.js or index.android.js

```
'use strict';

import {
  AppRegistry,
  Component
} from 'react-native';

import {InitUtil} from 'easier-react-native';

class Store extends Component {
  render() {
    return InitUtil.init('Login');
  }
}

AppRegistry.registerComponent('Store', () => Store);
```

##自定义控件

###NavBar

**来源：** <https://github.com/react-native-fellowship/react-native-navbar>

在原有的基础上做了细微的改动和bug修复，感谢原作者

**API**

```
style - (Object, Array) - Style object or array of style objects
    tintColor - (String) - NavigationBar's tint color
    statusBar - (Object):
        style - ('light-content' or 'default') - Style of statusBar
        hidden - (Boolean)
        tintColor - (String) - Status bar tint color
        hideAnimation - ('fade', 'slide', 'none') - Type of statusBar hide animation
        showAnimation - ('fade', 'slide', 'none') - Type of statusBar show animation
    leftButton / rightButton - (Object, React Element) - Either plain object with configuration, or React Element which will be used as a custom left/right button element. Configuration object has following keys:
        title - (String) - Button's title
        tintColor - (String) - Button's text color
        style - (Object, Array) - Style object or array of style objects
        handler - (Function) - onPress function handler
    title - (Object, React Element) - Either plain object with configuration, or React Element which will be used as a custom title element. Configuration object has following keys:
        title - (String) - Button's title
        tintColor - (String) - Title's text color
```

---

###TitleBar

自定义头部控件

**API**

```
extends NavBar API:
   style - (Object, Array) - Style object or array of style objects
   tintColor - (String) - NavigationBar's tint color
   statusBar - (Object):
       style - ('light-content' or 'default') - Style of statusBar
       hidden - (Boolean)
       tintColor - (String) - Status bar tint color
       hideAnimation - ('fade', 'slide', 'none') - Type of statusBar hide animation
       showAnimation - ('fade', 'slide', 'none') - Type of statusBar show animation
   leftButton / rightButton - (Object, React Element) - Either plain object with configuration, or React Element which will be used as a custom left/right button element. Configuration object has following keys:
       title - (String) - Button's title
       tintColor - (String) - Button's text color
       style - (Object, Array) - Style object or array of style objects
       handler - (Function) - onPress function handler
   title - (Object, React Element) - Either plain object with configuration, or React Element which will be used as a custom title element. Configuration object has following keys:
       title - (String) - Button's title
       tintColor - (String) - Title's text color

new API：
leftButton / rightButton:
	hidden - (Boolean) - hidden button
	extends Button all API：
		color - (string) - text color
		fontSize - (number) - text size
		style - (Object) - button background style objects
		imgStyle - (Object) - button image style objects
		pressSource - (source, Object) - button press source, image source or {color:''}
		source - (source, Object) - button source, image source or {color:''}
		onPress - (function) - button press listener
bottomLine - (any) - boolean: Whether show default bottom line
				 - string: show bottom line backgroundColor
				 - object: show bottom line style
				 - element: show bottom view
```

**Method**

- **setTitle(props:string/object)** - update title
- **setLeftButton(props:object/element)** - update leftButton
- **setRightButton(props:object/element)** - update rightButton
- **getTitleProps()** - get title current props
- **getLeftButtonProps()** - get leftButton current props
- **getRightButtonProps()** - get RightButtonProps current props

**应用：**

```
import {
	TitleBar
} from 'easier-react-native'

//...

<TitleBar
    style={styles.titlebar}
    tintColor=‘blue’
    statusBar={{
    	style: 'light-content'
    }}
    leftButton={{
    	title: 'test'
    	handler: () => {
    		console.log('click test');
    	}
    }}
    rightButton={(
    	<View style={{width: 20, height: 20, backgroundColor: 'red'}} />
    )}
    navigator={this.props.navigator}
/>
```

---

###Button

自定义Button控件

**API**

```
color - (string) - text color
fontSize - (number) - text size
style - (Object) - button background style objects
imgStyle - (Object) - button image style objects
pressSource - (source, Object) - button press source, image source or {color:''}
source - (source, Object) - button source, image source or {color:''}
onPress - (function) - button press listener
```


**应用：**

```
import {
	Button
} from 'easier-react-native'

//...

<Button
	color='white'
	style={{wieth: 70, height: 45}}
	imgStyle={{wieth: 70, height: 45}}
	source=require('./image/button.png')
	source=require('./image/button.png')
	onPress=() => {
		console.log('click test');
	} >
	Test Button
</Button>
```

---

###Loading

加载Loading控件

**API**

```
text - (string) - loading text
textStyle - (object) - loading text style
pointerEvents - (bool) - loading can click on the bottom of the content, default is false
bottomStyle - (object) - loading the bottom cover background style
loadingStyle - (object) - loading background style
```

**Method**

- **show(text:string, pointerEvents:bool)** - show loading
- **dismiss()** - dismiss loading
- **isShow()** - return loading is showed

**应用：**

```
import {
	Loading
} from 'easier-react-native'

//...

render() {
	return (
		<loading ref='loading' text='登录中...' />
	);
}

getLoading() {
	return this.refs['loading'];
}

test() {
	this.getLoading().show();
	this.getLoading().show('注册中', false);
	this.getLoading().dismiss();
}

```

---

###CircleProgress

圆形进度条

```
import {
	CircleProgress
} from 'easier-react-native'

//...

<CircleProgress />
```

---

###ViewStack

**说明：** Component容器控件，可放置一个Component的数组加入Stack，Stack中的Component只有调用replaceStack()方法才执行render()渲染界面

**API**

```
stack - ([Component]) - ViewStack stack array, must be a Component array
```

**Method**

- **replaceStack(index:number, isNew:bool)** - Replace the component of the specified index in the stack，isNew said to create a new instance
- **getCurrentIndex()** - The index to get the current display
- **getStack(index:number)** - Specify the subscript component instance


**应用：**

```
import {
	ViewStack
} from 'easier-react-native'

//...

render() {
	return (
		<ViewStack ref='stack' style={styles.content} stack={[<Home />, <Manage />]} />
	);
}

_getStack() {
    return this.refs['stack'];
}

test() {
	this._getStack().replaceStack(1, false);
	let index = this._getStack().getCurrentIndex();
	let component = this._getStack().getStack(1);
}

```


---

###TabBar

tab切换控件

**API**

```
tabItems - ([object/element]) - TabBar item, Can be the object or element of the array
    if object:
        style - (object) - item style
        title - (string) - item title
        titleColor - (string) - item title color
        titleSelected - (string) - item title selected color
        image - (any) - item ico
        selectedImage - (any) - item selected ico
tintColor - (string) - TabBar backgroundColor
style - (object) - TabBar style
handler - (func) - TabBar tab change listener
index - (number) - TabBar init selected index
```

**Method**

- **getCurrentIndex()** - return current selecter item index

**应用：**

```
import {
	TabBar,
	ViewStack
} from 'easier-react-native'

//...

constructor(props) {
    super(props);

    this.tabItems = [
        {title: '首页', titleColor: 'blank', titleSelected: 'red', image: require('../image/xxx.png'), selectedImage: require('../image/xxx.png')},
        (<View style={{width: 20, height: 20, backgroudColor: 'yellow'}} />),
        {title: '管理', titleColor: 'blank', titleSelected: 'red', image: require('../image/xxx.png'), selectedImage: require('../image/xxx.png')},,
    ];

}

renderBody() {
    return (
        <View style={styles.container}>
            <ViewStack ref='stack' style={styles.content} stack={[<Home />, <Manage />]} />
            <TabBar {...this._tabBarConfig()}/>
        </View>
    );
}

_getStack() {
    return this.refs['stack'];
}

_tabBarConfig() {
    return {
        tabItems: this.tabItems,
        tintColor: 'white',
        index: 0,
        handler: (index) => {
            this._getStack().replaceStack(index);
            this.getTitleBar().setTitle(this.tabItems[index].title);
        }
    };
}

```

---

##HTTP请求

###FetchUtil

网络请求工具类

**Method**

- **init()** 初始化工具类
- **setUrl(url:string)** 设置请求URL
- **setMethod(method:string)** 设置请求方式，`'GET'/'POST'/'PUT'/'DELETE'`，默认`'GET'`
- **setBodyType(type:string)** 设置请求body类型，`'form'/'file'/'json'`，默认`'form'`
- **setReturnType(type:string)** 设置返回data类型，`'json'/'text'/'blob'/'formData'/'arrayBuffer'`，默认`'json'`
- **setOvertime(time:number)** 设置超时时间，毫秒
- **setHeader(name:string/object, value:string)** 设置Header，name若为字符串，则name和value为header键值对数据，若name为object，则name为header键值对对象
- **setBody(name:string/object, value:string)** 设置请求body，参数同上
- **thenStart(then:function)** 设置请求成功后第一个回调方法then，通常用于处理网络返回的第一笔数据，需要将此对象return出去，交由后面的then处理
- **dofetch()** 执行请求

**Example**

```
import {
	FetchUtil,
} from 'easier-react-native'

//工具类实例可重用，建议一个实例化一次之后复用
let fetchUtil = new FetchUtil();

fetchUtil.init()
	.setUtl('http://gank.io/api/random/data/Android/20')
	.setMethod('GET')
	.setOvertime(30 * 1000)
	.setHeader({
	    'Accept': 'application/json',
	    'Content-Type': 'application/json',
	    'DEVICE-ID': 'iphone6',
	    'SYSTEM': 'ios/android',
	})
	.setBody('name', 'test')
	.dofetch()
	.then((data) => {
		console.log('=> data: ', data);
	})
	.catch((error) => {
		console.log('=> catch: ', error);
	});

```

---

###LtFetch

此类为根据本人公司API协议逻辑封装，建议大家都根据项目API协议，在封装一个工具类
，此类供参考

```
'use strict';

import FetchUtil from './FetchUtil';

class LtFetch extends FetchUtil {

	constructor(baseUrl = '') {
		super();
		this.baseUrl = baseUrl;
 	}

	dofetch() {
        this.url = this._formatUrl(this.url, this.bodys);
		let logBody = '';
		if (this.method != 'GET') {
			logBody = this.bodys;
		}
		console.log(`\n=> fetch:\n\turl:${this.url}\n`, logBody ? '\tbody:' : '', logBody ? logBody : '\n');

		this.thenStart(
			(response) => {
				this.checkStatus(response);
				return response;
			}
		);

		let p = super.dofetch()
			.then((data) => {
				console.log(`\n=> data:`, data, '\n\n');
				return data;
			})
			.catch((err) => {
				if (err.message == 'request timeout') {
					err = -998;
				}
				if (err.message == 'request cancel') {
					err = -1000;
				}
				console.log(`\n=> catch:`, err, '\n\n');
				throw err;
			});

		return p;
	}

	init() {
		this.isCheckStatus = true;
		super.init();
		return this;
	}

	checkStatus(response){
		if (this.isCheckStatus && response.headers.map['api-status'] != 1) {
			throw parseInt(response.headers.map['api-status']);
		}
	}

	setCheckStatus(isCheckStatus) {
		this.isCheckStatus = isCheckStatus;
		return this;
	}

	//如果api中带有{}携带参数格式化，拼接host，如：/Cats/{id}/{page}
    _formatUrl(url, params) {
        if (url.includes('{') && !!params) {
			let names = Object.keys(params);
        	for (let i = names.length-1; i >= 0 ; i--) {
				let name = names[i];
        		if (url.includes(name)) {
        			url = url.replace('{'+name+'}', params[name]);
        		}
        	}
        }
		return url.startsWith('http://') ? url : this.baseUrl + url;
    }

}

module.exports = LtFetch;
```

**Example**

```
import {
	LtFetch,
} from 'easier-react-native'

//工具类实例可重用，建议一个实例化一次之后复用
lt host = 'http://api.91kkxiong.net:8080';
let fetchUtil = new LtFetch(host);

fetchUtil.init()
	.setUtl('/shop/main')
	.setMethod('GET')
	.setHeader({
	    'API-VERSION': '1',
	    'Accept': 'application/json',
	    'Content-Type': 'application/json',
	    'DEVICE-ID': 'iphone6',
	    'SYSTEM': 'ios/android',
	    'APP-NAME': 'cwsq',
	    'ACCESS-TOKEN': '',
	})
	.dofetch()
	.then((data) => {
		console.log('=> data: ', data);
	})
	.catch((error) => {
		console.log('=> catch: ', error);
	});

```
