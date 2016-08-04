#1、单例模式#
  
通过一个函数返回唯一实例。  
**Example**
<pre><code>var singleton = function( fn ){
    var result;
    return function(){
        return result || ( result = fn .apply( this, arguments ) );
    }
}
 
var createMask = singleton( function(){
 
return document.body.appendChild( document.createElement('div') );
 
 })
</code></pre>  
#2、工厂模式#

通过一个方法来决定到底是要创建哪个类的实例。  
**Example**
<pre><code>function A( name ){
    this.name = name;
}
function ObjectFactory(){
    var obj = {},
        //shift方法取得数组中的第一个元素并删除。
	Constructor = Array.prototype.shift.call( arguments );
    obj.__proto__ =  typeof Constructor.prototype === 'number'  ? Object.prototype:  Constructor.prototype;

    var ret = Constructor.apply( obj, arguments );
 
    return typeof ret === 'object' ? ret : obj;
 
}
 
var a = ObjectFactory( A, 'lei' );
 
console.log( a.name );
</code></pre>
#3、观察者模式#

俗称为发布者-订阅者模式，主要解决于两个事件的解耦。  
**Example**
<pre><code>function event() {
    var __this, obj, listen, one, remove, trigger;
    listen = (key, eventFn) => {
        var stack, _ref;
        stack = (_ref = obj[key]) != null ? _ref : obj[key] = [];
        stack.push(eventFn);
    };
    one = (key, eventFn) => {
        remove(key);
        listen(key, eventFn);
    };
    remove = (key) => {
        var _ref;
        return (_ref = obj[key]) != null ? _ref.length = 0 : void 0;
    };
    trigger = () => {
        var stack, _ref, key;
	key = Array.prototype.shift.call(arguments);
	stack = (_ref = obj[key]) != null ? _ref : obj[key] = [];
	for(var fn of stack) {
	    if(fn.apply(__this, arguments) === false) {
		return false;
	    }
	}
    }
    return {
	listen : listen,
	one : one,
	remove : remove,
	trigger : trigger
    }
}
</code></pre>
#4、适配器模式#

适配器模式作用很像是一个转接口，将现有的接口包装一下拿来用。  
**Example**
<pre><code>function returnString(string1, string2) {
    return "You are" + string1 + string2;
}
function toReturnString(object) {
    return returnString(obj.string1, obj.string2);
}
var string = toReturnString({
        string1 : "my",
	string2 : "friend"
    });
</code></pre>
#5、代理模式#

代理是一个对象，它可以用来控制对另一个对象的访问。  
**Example**
<pre><code>function parents() {
    this.son = "lei";
}
parents.prototype = {
    getSonName() {
	return this.son;
    }
}
function parent() {
    this.parent = null;
}
parent.prototype = {
    _init() {
	if(this.parent === null){
	    this.parent = new parents();
	}
    },
    getSonName() {
	this._init();
	return this.parent.getSonName();
    }
}
</code></pre>
#6、桥接模式#

桥接模式的作用在于把实际的部分和抽象的部分分离。  
**Example**
<pre><code>forEach = (array, fn) => {
    for(var i = 0; i < array.length; i++) {
	var item = array[i];
	if(fn.call(item, i, array) === false) {
	    return false;
	}
    }
}
forEach(["a", "b"], (key, index, array) => {
    console.log(key);
});
</code></pre>
#7、外观模式#

外观模式又称门面模式，是提供一个高层接口，使使用起来更加方便。  
**Example**
<pre><code>function setStyle(elements, styles) {
    for(var item of elements) {
	var element = document.getElementById(item);
	for(var property in styles){
	    element[property] = styles[property];
	}
    }
}
setStyle(["a", "b"], {
    color : "red"
});
</code></pre>
#8、访问者模式#

访问者模式就是先把一些可服用的行为抽象到一个函数(对象)里，这个函数我们就称为访问者  
**Example**
<pre><code>function arrayPush(object, pushObject) {
    var length;
    if(object instanceof Array) {
	length = object.length;
    } else {
	length = Obejct.keys(object).length;
    }
    for(var i = 0; i < pushObject.length; i++) {
	object[i + length] = pushObject[i];
    }
    return object;
}
</code></pre>
#9、策略模式#
策略模式又叫算法簇模式，就是定义了不同的算法，并且之间可以相互替换，此模式让算法的变化独立于使用算法的客户。  
**Example**
<pre><code>var dataSourceVendor = {
    xml : {
	get() {
	    console.log("XML数据");
	}
    },
    json : {
	get() {
	    console.log("json数据");
	}
    },
    jsonp : {
	get() {
	    console.log("jsonp数据")
	}
    }
}
console.log（"选择的数据格式： " + dataSourceVendor["json"]["get"]）;
</code></pre>
#10、模板方法模式#
模版方法是预先定义一组算法，先把算法的不变部分抽象到父类，再将另外一些可变的步骤延迟到子类去实现。  
**Example**
<pre><code>function gameCenter() {}
gameCenter.prototype = {
    init() {
	this.login();
	this.gameStart();
	this.end();
    },
    login() {},
    gameStart() {},
    end() {}
};
var game = function() {};
game.prototype = gameCenter.prototype;
game.prototype.gameStart = () => {
    console.log("游戏开始了!");
};
(new game).init();
</code></pre>
#11、中介者模式#

中介者对象可以让各个对象之间不需要显示的相互引用，从而使其耦合松散，而且可以独立的改变他们之前的交互。  
**Example**
<pre><code>function duijiangji(name) {
    this.name = name;
}
duijiangji.prototype = {
    send(msg, object) {
	people.send(msg, object);
    },
    jieshou(msg) {
	console.log(this.name + "接收到：" + msg);
    }
};
var people = {
    all : {},
    create(object) {
	this.all[object.name] = object;
    },
    send(msg, object) {
	this.all[object.name].jieshou(msg);
    }
};
var ren1 = new duijiangji("小四"),
    ren2 = new duijiangji("小七");
people.create(ren1);
people.create(ren2);
ren1.send("我看见你了", ren2);
</code></pre>
#12、迭代器模式#

迭代器模式提供一种方法顺序访问一个聚合对象中各个元素，而又不需要暴露该方法中的内部表示。  
**Example**
<pre><code>function Iterator(arr) {
    this.iterator = arr;
    this.length = 1;
    this.index = 0;
    if(arr instanceof Array){
	this.length = arr.length;
    } else {
	for(var key in arr) {
	    this.length++;
	}
    }
}
Iterator.prototype = {
    first() {
	return this.iterator[0];
    },
    last() {
	return this.iterator[this.length - 1];
    },
    hasNext() {
	return this.index < this.length;
    },
    next() {
	var data = this.iterator[this.index];
	this.index++;
	return data;
    }
};

var iterator = new Iterator([3, 6, 9]);
var iterator2 = new Iterator({0 : 1, 1 : 2});

console.log(iterator.first());
console.log(iterator.last());
console.log(iterator.hasNext());
console.log(iterator.next());
</code></pre>
#13、组合模式#

组合模式又叫部分-整体模式，它将所有对象组合成树形结构，使得用户只需要操作最上层的接口，就可以对所有成员做相同的操作。  
**Example**
<pre><code>function company(name) {
    this.name = name;
    this.departments = [];
}
company.prototype = {
    constructor : company,
    addDepartment(dep){
	this.departments.push(dep);
	return this;
    },
    getDepartments() {
	return this.departments;
    }
}
function department(name) {
    this.name = name;
    this.presons = [];
}
department.prototype = {
    constructor : department,
    addPerson(person) {
	this.persons.push(person);
	return this;
    },
    getPersons() {
	return this.persons;
    }
}
function person(name) {
    this.name = name;
}
person.prototype = {
    contructor : person,
    goWork() {
	console.log(this.name + "赶紧去工作!");
    },
    goSleep() {
	console.log(this.name + "可以休息了!");
    }
}
var companyTest = new company("测试公司"),
    departmentTest = new department("测试部门"),
    person = new person("小七");
departmentTest.addPerson(person);
companyTest.addDepartment(departmentTest);
for(var dep of companyTest.getDepartments) {
    for(var per of dep.getPersons) {
	if(per.name === "小七") {
	    per.goSleep();
	}
    }
}
</code></pre>
#14、备忘录模式#

备忘录模式在js中经常用于数据缓存。
**Example**
<pre><code>var page = function() {
    var page = 1,
	cache = {},
	data;
    return function(page) {
	if(cache[page]) {
	    data = cache[page];
	    console.log(data);
	} else {
	    find('xxxx', (data) => {
		cache[page] = data;
		console.log(data);
	    })
	}
    }
}();
</code></pre>
#15、职责链模式#

职责链模式是一个对象A向另一个对象B发起请求，如果B不处理，可以把请求转给C，如果C不处理，又可以吧请求转给D，一直到有一个对象愿意处理这个请求为止。  
**Example**
<pre><code>fucntion handle(o, t) {
    this.object = o || null;
    this.top = t || 0;
}
handle.prototype = {
    handle() {
	if(this.object) {
	    this.object.handle();
	}
    },
    has() {
	    return this.topic >= 0;
	}
};

var app = new handle({
    handle() {
	console.log("app handle");
    }
});
var one = new handle(app, 1);
var two = new handle(one, 2);

two.handle();
</code></pre>
#16、享元模式#

享元模式主要用来减少程序所需的对象个数。  
**Example**
<pre><code>function Car(make, model, year) {
    this.make = make;
    this.model = model;
    this.year = year;
}
Car.prototype = {
    getMake() {
	return this.make;
    },
    getModel() {
	return this.model;
    },
    getYear() {
	return this.year;
    }
}
</code></pre>
#17、状态模式#

状态模式主要可以用于的场景  
<ol>
<li>一个对象行为取决于它的状态
<li>一个操作中含有庞大的条件分支语句
</ol>
**Example**
<pre><code>var stateManager = StateManager();
stateManager.changeState('jump');
function StateManager() {
    var currState = 'wait';
    var states = {
	jump(state) {
	    currState = 'states';
	},
	wait() {
	    currState = 'wait';
	},
	attack() {
	    currState = 'attack';
	},
	crouch() {
	    currState = 'crouch';
	},
	defense() {
	    currState = 'defense';
	}
    };
    var changeState = function(state) {
	states[state] && states[state]();
    }
}
</code></pre>