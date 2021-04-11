##### Table of Contents 
  * [LifeCycle](#lifecycle)
  * [Reactivity system](#reactivity-system)
  * [Standardise Tool use in VueJs](#standardise-tool-use-in-vuejs)
  * [SFC (Single File Component)](#sfc--single-file-component-)
  * [directive hook](#directive-hook)
  * [v-directive](#v-directive)
    + [v-model](#v-model)
    + [v-model.[modifier] - trim,number,lazy, capitalize](#v-model-modifier----trim-number-lazy--capitalize)
    + [v-bind](#v-bind)
    + [@click.[modifier] (v-on:)](#-click-modifier---v-on--)
    + [@change (trigger when user leaves the input)](#-change--trigger-when-user-leaves-the-input-)
    + [@input (onChange)](#-input--onchange-)
  * [@keydown.enter](#-keydownenter)
    + [v-pre](#v-pre)
  * [data() 是 return Object](#data-----return-object)
  * [Loop](#loop)
  * [$attrs](#-attrs)
  * [ref](#ref)
  * [Reactive](#reactive)
  * [toRefs and reactive](#torefs-and-reactive)
  * [slot (children)](#slot--children-)
  * [Options API](#options-api)
  * [Composition API](#composition-api)
  * [Props passing from both component](#props-passing-from-both-component)
  * [this object in methods](#this-object-in-methods)
    + [$emit](#-emit)
    + [$route](#-route)
  * [<router-view> & routes](#-router-view----routes)
  * [onErrorCaptured](#onerrorcaptured)
  * [Suspense](#suspense)
  * [defineAsyncComponent](#defineasynccomponent)
  * [Computed()](#computed--)
  * [Watcher (options API)](#watcher--options-api-)
  * [Watcher (reactive)](#watcher--reactive-)
  * [Provide/Inject](#provide-inject)
  * [Where to call API in lifeCycle](#where-to-call-api-in-lifecycle)
  * [Mixins](#mixins)
  * [Listen route change](#listen-route-change)
    + [beforeRouteLeave(to, from, next)](#beforerouteleave-to--from--next-)
    + [beforeRouteEnter](#beforerouteenter)
  * [Vue delimiters](#vue-delimiters)
  * [Transition Component](#transition-component)


## LifeCycle

   `LifeCycle methods`       `after lifeCycle step what vue to do`

1. beforeCreate             → then Observe data & initialize events
2. created                      → then Compile template (created後已經init有data和props的data了）
3. beforeMount             → then Create vm.$el & update DOM
4. mounted                    → then `Mounted`了， 如果有 Data Changed
5. beforeUpdate            → Re-render virtual DOM & patch
6. updated                      → `Mounted`

    `Destroy LifeCycle`

                                            → when vm.$destroy() is called

1. beforeDestroy        → remove watchers, components and event listeners.

                                       `Destroyed`

 2.  destroyed

## Reactivity system

Vue用這個system，所以他會知道 哪個state被更改 然後只re-render那個component

1. Vue把所有的對象 都用 `Object.defineProperty` 轉換去 `gettter/setter`
2. 基於內部機制 可以使 Vue 在属性被访问和修改时去触发相应的 getter 和 setter，以实现dependency追踪(dependency-track)和变更通知(change-notification
3. 當在 data() { return { name: ''“ } } 時， Vue `initialise`就會`執行` `step2`了，所以我們才不能+新的 data 如果 initialise 時沒有的話， 那樣 它就不會reactivity了(不能紀錄在vue裡)
4. 每个组件实例都有相应的 watcher 实例对象，它会在组件渲染的过程中把属性记录为`dependency`，之后当`dependency`项的 setter 被调用时，会通知 watcher 重新计算，从而致使它关联的组件得以更新。(trigger setter to notify the watcher)

## Standardise Tool use in VueJs

1. Vue Router 
2. Vuex - state management 
3. Vue test utils
4. Vue devtools - debuging 
5. vee-validate & yup

## SFC (Single File Component)

```html
<template></template>

<script></script>

<style></style>
```

## directive hook

1. **bind** – called once when the directive is bound to an element
2. **inserted** – when the bound element is inserted into its parent node
3. **update** – when the element updates (but any children haven’t yet)
4. **componentUpdated** – after the children have also updated
5. **unbind** – called once when the directive is unbound from an element

## v-directive

1. v-model
2. v-bind:href="url" - `:href`
3. v-for
4. v-on:click - `@click`
5. @dblclick=“”
6. @sumbit.prevent="fnName"
7. v-if (比較貴 因為他是insert新的dom 建議用在不常更新的state)
8. v-show (dom 不會被移除 他是 css display control，所以比較不貴)
9. v-else (上面要是 v-if 才行)
10. @mouseover=“methodName” method裡的第一個 (e) 就是e. 它會自動pass，不需要 在上面define
11. @mouseleave=“methodName($event, 5)” 如果有pass args 就需要 寫event 如果你在 method裡要用event的話啦

### v-model

1. mainly for input and form binding, so use it when you dealing with various input types
2. v-model is for two way bindings means: if you change input value, the bound data will be changed and vice versa. (如果你更改data 最上面的origin data也會被更改)

### v-model.[modifier] - trim,number,lazy, capitalize

```html
<input type="text" v-model.trim="formValues.name" \>

// 沒有馬上update 需要按出去input才update
<input type="text" v-model.trim.lazy="formValues.name" \>
```

### v-bind

1. allow you to produce some dynamic value by typing some JS expression that in most cases depends on the data from data model - so think about v-bind as directive that you should use when you want deal with some dynamic things
2. v-bind:value is called one way binding that means: you can change input value by changing bound data but you can't change bound data by changing input value through the element(更改data 只會更改當前的data 他不會影響到 最原始的data, 大多數是 `bind`在 `html attribute, custom props`)

### @click.[modifier] (v-on:)

> v-on directive is how you run JavaScript in response to DOM events. If you want to run some code when the user clicks a button, you should use v-on

```html
<template>
	<div class="overlay" @click.self="closeModal">
		<div class="modal">
			<p>Some modal content...</p>
			<p>雖然 overlay是 parent 但是 click.self的功能是 只有點擊到那個layout才trigger
				closeModal，點擊到 它的children modal 是不會trigger到這個function的
				它不是bubbling的 dom了
			</p>
		</div>
	</div>
</template>
```

### @change (trigger when user leaves the input)

```jsx
<input @change="callthisValidateFn" v-model="value" />
```

### @input (onChange)

## @keydown.enter

```jsx
<input @keydown.enter="clickEnterCallFn" />
```

### v-pre

```html
<div v-pre>{{renderAsStringNotParseToVue}}</div>
```

## data() 是 return Object

```jsx
export default {
	data() {
		return { 
				items: [],
				formValues: { name: '', age: ''}
		 }	
	},
	// 也可以寫成
	data: {
		items: [],
		formValues: { name: '', age: '' }
	}
}
```

## Loop

```jsx
const app = new Vuew({
  el: "#app",
	data: {
		todos: [
	    {text: "string 1"},
	    {text: "string 2"},
	    {text: "string 3"},
    ]
  }
})
```

```html
<div id="app">
	<ol>
		<li v-for="todo in todos">
			{{ todo.text }}
		</li>
	</ol>
</div>
```

## $attrs

```jsx
// parent
/*
	By default it will pass down to the first div
	child component 也不需要 set props， 它會自動pass
	但是 如果你不要去第一個div呢？
	需要用 attrs
*/
<template>
	<Child :color="bgColor" id="12" /> 
</template>

// Child
<template>
	<div>
		<div v-bind=“$attrs“ :class="$attrs.bgColor"></div>
	</div>
</template>

export default {
	inheritAttrs: false,
}
```

## ref

```html
<template>
	<input type="text" ref="somename" />
	<button @click="handleClick">Cick me</button>
</template>

<script>
export default {
	name: 'App',
	methods: {
    handleClick() {
      console.log(this.$refs.somename)
    }
  }
}
</script>
```

```jsx
import {ref, onMounted} from 'vue';
// 在 Composition API裡 需要 ref value 不然 他不會 reactivity in vue system
export default {
	setup() {
		const somename = ref(null);

		onMounted() {
			somename.value.focus();
		}
	}
}
```

## Reactive

```jsx
import { reactive, toRef } from 'vue';

/**
	不能接受 Primitive in reactive
	1.Only accept Object {} then it will reactivity 
*/
export default {
	const obj = reactive({ a: 'value', b: 'cool' });
	const aRef = toRef(obj, "a"); // Destructure it
	
}
```

## toRefs and reactive

```jsx
// composables folder | hook f
import { reactive, toRefs } from 'vue';

const usePosts = () => {
	const state = reactive({
		error: null,
		posts: null,
	});

	const load = async () => {
		try {
			let data = await fetch("https://');
			if(!data.ok) return throw Error("No data available");
		
			state.posts = await data.json();
		} catch (err) {
			state.error = err.message;
		}
	}

	return {
		/*
			 在 Template 直接 {{ posts }} 就行了 不需要 state.posts， 因為 toRefs的關係
			 如果是 return ｛ state } 
			 template 需要 state.posts那樣就沒有問題
			 但是每個都需要 state. 很麻煩
			 return { firstName: state.firstName, }
			 那樣的話 更改 posts 會 update 不到 view 因為 vue不知道 
			 state裡面的單獨的value被換了
			 所以需要 return toRefs(state) 
			 Template 上面一樣 不需要寫 state. 直接 posts
		*/
		...toRefs(state), 
		load,
	}
}
```

## slot (children)

```html
// parent
<Modal>
	<template v-slot:nameSlot>
		<p>這個也是child 但是它會根據 vue的directive去放他的位子</p>
  </template>
	<p>some text</p>
<Modal>

// Modal Component
<template>
	<div>
		// 上面的 p tag children 會放進這個 slot
		<slot></slot>
		<div>
			// 這裡的slot 是 上面 Modal child裡的template會放進這裡
			<slot name="nameSlot"></slot>
    </div>
	</div>
</template>

```

## Options API

```jsx
export default {
	data() {},
	mounted() {},
	methods() {},
	coumputed() {},
}
```

## Composition API

```jsx
// Composition API 這些 method 都要自己+的 
import {computed, ref, reactive} from 'vue';

export default {
	props: ['propsName'],
	setup(props) {
		/*
			1. 需要用ref 去 bind data
			2. 不能用 this 在 setup裡
		*/
		const { a, b } = toRefs(props) // destructure 

		// data
		// mounted
		// methods
		// lifecycle hooks
  }
}
```

1.  大型project比較好mantain
2. 它的code 可以 share用

## Props passing from both component

```jsx
// parent
<template>
	<input type="text" v-model="firstName" />
	<input type="text" v-model="lastName" />
	<ChildComponent :firstName :lastName @AnyName="callFn" />
</template>

export default {
	name: 'ParentComponent',
	components: { ChildComponent },
	setup() {
		const firstName = ref('');
		const lastName = ref('');

		function callFn(value) {
			console.log(`value passing from child ${value}`)
		}

		return { firstName, lastName }
	},
}

// child
<template>
	<button @click="sendToParent">Call Heroes</button>
</template>

export default {
	name: 'ChildComponent',
	setup(props, context) {
		const fullName = computed(() => `${props.firstName} ${props.lastName}`)
		
		// use regular function, the "this" is bind in this instance
		function sendToParent() {
			context.emit("AnyName", fullName.value);
		}
	},
	emits: ["AnyName"], // same as Options Api when use in Compisition API
	// Options API Way | composition APi need it too
	props: ['firstName', 'lastName']
}
```

## this object in methods

### $emit

```jsx
// just a note, we can call emit in function too
export default {
	name: 'Child',
	methods: {
		someFunctionName() {
			// logic below
			const result;
			this.$emit('AnyName', result) // we call $emit from this object
		}
	}
}
// parent
<template>
	<Child @AnyName="handleFn" />
</template>
```

### $route

```jsx
export default {
	name: 'Note',
	methods: {
		showSomeFn() {
			// We-Only-Can-Push-Params-In-Object
			this.$route.push({ name: 'RouteName', params: { id: 2 } })
			// we can get value from params
			const { id } = this.$route.params
		}
	}
}
```

## <router-view> & routes

```jsx
const routes = createRouter({
	history: createWebHistory(),
	routes: [
		{
			path: '/',
			name: '',
			components: '',
			meta: {
				someKeyName: 'value', // we can access this from router-link tag
			}
		}
	]
})

// route.meta can retrieve the value you set in routes
<router-view v=slot={ Component, route }>
	<component :is="Component" /> // dynamic component
</router-view>
```

## onErrorCaptured

```jsx
import { onErrorCaptured, ref } from "vue";

export default {
	setup() {
		const error = ref(null);

		onErrorCaptured(e => {
			error.value = e;
			return true;
		});

		return { error }
	},
}

<template>
	<div v-if="error">Error: {{ error }}</div>
</template>
```

## Suspense

```jsx
// parent
<template>
	<Suspense> 
		<template #default><User /></template>
		<template #fallback><div>loading...</div></template>
	</Suspense>
</template>

// Suspense 會去 fallback 當 User component 的 aysnc 還沒有 return回來

// child
<template>
	<div>{{ result }}</div>
</template>
export default {
	async setup() {
		const result = await axios("https://");
		return { result }
	}
}
```

## defineAsyncComponent

```jsx

import { defineAsyncComponent } from "vue";
import Spinner from "@/components/Spinner.vue";

const ChatWindow = defineAsyncComponent({
  loader: () => import("@/components/ChatWindow"),
  loadingComponent: Spinner,
  delay: 3000
});

export default {
  components: {
    ChatWindow
  },
  ...
}
```

## Computed()

```jsx
// 當 UI re-render 就會call 到 getTotal，而computed 如果dependency 沒更改 他不會re-render它

<template>
	<button @click="changeFullName">Change computed value</button>
</template>

export default {
  data(){
    return {
      items:[],
			fistName: "yee",
			lastName: "soh",
    }
  },
  methods: {
    getTotal(){
      console.log('getTotal methods called')
    },
		changeFullName() {
			// fullName 是 指向 computed裡的object
			this.fullName = "laoyeche Wilker" // 這個 value 是 set的(value)
		}
  },
	 computed: {
    total() {
      console.log('total computed') 
      return this.items.reduce((total, curr) => (total = total + cur.price), 0)
    },
		fullName: {
			get() {
				return `${this.firstName} ${this.lastName}`
			},
			set(value) {
				const names = value.split(" ");
				this.firstName = names[0];
				this.lastName = names[1];
			}
		}
  }
}

// in setup() way
export default {
	setup() {
		const isLoading = computed(() => !list.data.length);
			
		return { isLoading } 
	}
}
```

```jsx
export default {
	setup() {
		const state = reactive({
			pokemons: [],
			filteredPokemon: computed(() => updatePokemon),
			text: '',
		});

		function updatePokemon() {
			
			return state.pokemons.filter(pokemon => pokemon.name.includes(state.text))
		}

		return {
			...toRefs(state)
    }
	}
}
```

## Watcher (options API)

```jsx
// when to use
// property changed to a favorable value then to do something
// Call An API in response to change in application data
// have to apply transition

export default {
	data() {
		return {
			volume: 0,
			movie: "",
			movieInfo: {
				name: '',
				actor: '',
			},
			movieList: ["laoyeche", "wilker"]
		}
	},
	watch: {
		/*
			watchVariableName(defaultNewValue, defaultOldValue)
			第一第二參數 都是default的 
			上面不需要pass， 只要dependency不一樣了 那他就是自動在newValue
		*/
		volume(newValue, oldValue) { // 這個volume是指向 data裡的 volume key來的
			if(newValue > oldValye && newValue === 16) {
				console.log("high volume will hurt your hearing")
			}
		},
		movie: { // 注意 這個是 obj, 因我 obj 我們可以放另外一個叫做 immediate的key
			handler(newValue) {
				console.log('handler name is provided from vue')
			},
			immediate: true, // 這個是 mounted 馬上call handler function
		}
		movieInfo: {
			handler(newValue) {
				console.log('Watcher is not watch nested item, unless deep key is true');
			},
			deep: true,
		},
		movieList: {
			handler(newValue) {
				console.log('Array also same, watcher cannot watch it unless deep key is true');
			},
			deep: true, // 可以不用 true， 只要 concat array就行了，它是新array， 
			//如果用 push， 那就是 modified 那樣就需要把 deep： true
		}
	}
}

<template>
	<button @click="volume++">++++</button>
	<button @click="volume--">--</button>
	<input type="text" v-model="movie" />
</template>
```

## Watcher (reactive)

```jsx
export default {
	name: "componentNme",
	setup() {
		const state = reactive({
			fName: '',
			lName: '',
			options: { // array or object 都要 deep true
				heroName: '',
			},
		});
	
		//  這屆 watch lName or fName
		watch(() => {
			// 我們需要 return state 去copy 多一分 object 
			// 不然 oldValue 會和 newValue 是一樣的
			return { ...state } // 這個 return 會影響到下面的 newValue value
		}, (newValue, oldValue) => {
			console.log(newValue.fName)
		});

  	// watch 單獨一個
	  watch(() => state.fName, (newValue, oldValue) => {
	  	console.log("if watch state independent value need callback the state value")
  		console.log("Doesn't need newValue.fName since cb state.fName", newValue)
  	})

		// watch reactive object or array (deep: true)
		// lodash _ > _.cloneDeep(state.options)
		watch(() => JSON.parse(JSON.stringify(state.options)), (newValue, oldValue) => {
			console.log('need to deepClone the state if not old with same with newValue')
		})
  
		return  {
			...toRefs(state),
		}
	}
}
```

## Provide/Inject

```jsx
/*

App holding data, need to pass down nested component and itself.
                              App
                               |
|-------------------------------------------------------------|
Component_A                Component_B                  Component_C
                                |                             |
                           Component_D                  Component_E
                                                              |
                                                        Component_F    
                               
*/

<template>
	/* App */
	<h2>App Component use name instead of username {{ name }}</h2>
	<Component_C />
</template>

export default {
	name: 'App',
	components: { Component_C },
	data() {
		return {
			name: 'Wilker',
		}
	},
	// pass to App it self need to use function instead of provide object
	provide() {
		return {
			name: this.name,
		}
	},
	// provide object cannot be use in App component itself
	provide: {
		username: 'Wilker' 
	}
}
```

```jsx
<template>
/* Component_F */
	<h2>Component F username {{ username }} </h2>
</template>

export default {
	name: 'Component_F',
	inject: ['username'],
}
```

## Where to call API in lifeCycle

```jsx
export default {
	data() {
		return {
			errorMsg = "",
			posts: []
		}
	},
	created() {
		// 在這裡 就能 access reactive data and event了
		this.getPostList()
	},
	methods: {
		getPostList() {
			try {
				axios("https://")
			} catch (err) {
				this.errorMsg = err;
			}
		}
	}
}
```

## Mixins

```jsx
/*
	Click 和 Hover 用一樣的邏輯 
	可以用Mixins 
	- 如果 裡面 有自己 data() 然後 return 是 counter.js 的 variable
		那樣 那個 component 會 overwrite掉
*/
// ClickCounter.vue
// Note: 缺點！ 我們不知道 incrementCount and count 從哪裡來 如果 app做大
<template>
	<button @click="incrementCount">Click {{ count }}</button>
</temaplte>

import CounterMixin from 'path';

export default {
	name: 'ClickCounter',
	mixins: [CounterMixin]
}

// HoverCounter.vue 
import CounterMixin from 'path';

export default {
	name: 'HoverCounter',
	mixins: [CounterMixin]
}

// counter.js
export default {
	data() {
		return {
			count: 0,
		}
	},
	methods: {
		incrementCount() {
			this.count + 1;
		}
	}
}
```

## Listen route change

### beforeRouteLeave(to, from, next)

```jsx
export default {
	// if it is false, then it will pop up a confirm
	name: '',
	beforeRouteLeave(to, from, next) {
		if(false) {
			next(true);
		} else {
			const response = confirm("are you want to leave?");
			next(response)
		}
	}
}
```

### beforeRouteEnter

## Vue delimiters

```jsx
// Default: ["{{", "}}"] 所以 我們的 value 都 是 {{ username }}
// 可以換掉它

// version 2 
new Vue({
  delimiters: ['${', '}']
})
// version 3
Vue.createApp({
  delimiters: ['${', '}']
})
```

## Transition Component

```css
/* Default */
.enter-from { opacity: 0 };
.enter-to { opacity: 1 }; /* default 的值 可以不用放 */
.enter-active { transition: opacity 2s ease-in };

.leave-from {  };
.leave-to {  };
.leave-active {  };

/* add name */
.someName.enter-from {};

.someName.leave-from {};
```

```jsx
<transition name="someName">
	<div>I will be transition</div>
</transition>
```

```jsx
<transition-group tag="ul" name="list"></transition-group>
/* first mounted */
<transition-group appear>
	<div></div>
	<div></div>
</transition-group>
/* custom appear class */
<transition-group 
	appear
	appear-class="custom-appear-class"
  appear-to-class="custom-appear-to-class"
  appear-active-class="custom-appear-active-class"
></transition-group>
```

```jsx
/*
	mode: by default 他們是一起進行的
	(out-in | in-out)
	如果下面的 沒有 放 mode，當我拿掉 最後 一個 list item，它會顯示 v-else的
	但是 當我 拿掉 我也有 list-leave-from 的 transition 他也會執行
	然後 v-else 也會同時執行
	所以 放 out-in 就是說 我先執行 leave的 再做in的 
*/
<transition name="switch" mode="out-in">
  <div v-if="todos.length">
    <transition-group tag="ul" name="list" appear>
				/* some list */
     </transition-group>
  </div>
  <div v-else>No todo list!</div>
</transition>
```

```jsx
/* Transition javascript hook */
EnterHook: （life cycle > before-enter 先）
before-enter, enter, after-enter

LeavesHook: （life cycle）
before-leave, leave, after-leave

<transiton
	@before-enter="beforeEnter"
	@enter="enter"
>
</transiton>

<transiton-group
	appear
	tag="ul"
	@before-enter="beforeEnter"
	@enter="enter"
>
	<li v-for="(list, index) in lists" :data-index="index"></li>
</transiton-group>

<script>
	import gsap from "gsap";
	setup() {
	 const beforeEnter = () => { /* set initial value */ }

	 const enter = (el, done) => {
		// if just step el.style.opacity, it wouldn't transition effect
		gsap.to(el, {
			opacity: 1,
			y: 0, // translateY 來的
			duration: 3, // 3 sec
			onCompleted: done, // after duration finish call done if not 
							           // afterEnter will trigger 
			delay: el.dataset.index * 0.2
		});
	 }
		

	}
</script>
```

Setup

```bash
$ npm install -g @vue/cli
$ vue create project-name
$ winpty vue.cmd create hello-world (type this if step two arrow key not work)
```
