[LifeCycle](#LifeCycle)

[Reactivity system](#Reactivity-system)

[Standardise Tool use in VueJs](#Standardise Tool use in VueJs)

[v-(directive)](#v-(directive))

[LifeCycle](#LifeCycle)
[LifeCycle](#LifeCycle)
[LifeCycle](#LifeCycle)
[LifeCycle](#LifeCycle)
[LifeCycle](#LifeCycle)
[LifeCycle](#LifeCycle)
[LifeCycle](#LifeCycle)
[LifeCycle](#LifeCycle)
[LifeCycle](#LifeCycle)
[LifeCycle](#LifeCycle)


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
2. 基於內部機制 可以使 Vue 在属性被访问和修改时去触发相应的 getter 和 setter，以实现依赖追踪(dependency-track)和变更通知(change-notification
3. 當在 data() { return { name: ''“ } } 時， Vue `initialise`就會`執行` `step2`了，所以我們才不能+新的 data 如果 initialise 時沒有的話， 那樣 它就不會reactivity了(不能紀錄在vue裡)
4. 每个组件实例都有相应的 watcher 实例对象，它会在组件渲染的过程中把属性记录为`dependency`，之后当`dependency`项的 setter 被调用时，会通知 watcher 重新计算，从而致使它关联的组件得以更新。(trigger setter to notify the watcher)

## Standardise Tool use in VueJs

1. Vue Router 
2. Vuex - state management 
3. Vue test utils
4. Vue devtools - debuging 

## v-(directive)

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

### v-model.[modifier] - trim,number,lazy

```html
<input type="text" v-model.trim="formValues.name" \>

// 沒有馬上update 需要按出去input才update
<input type="text" v-model.trim.lazy="formValues.name" \>
```

### v-bind

1. allow you to produce some dynamic value by typing some JS expression that in most cases depends on the data from data model - so think about v-bind as directive that you should use when you want deal with some dynamic things
2. v-bind:value is called one way binding that means: you can change input value by changing bound data but you can't change bound data by changing input value through the element(更改data 只會更改當前的data 他不會影響到 最原始的data, 大多數是 `bind`在 `html attribute, custom props`)

### @click.[modifier]

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

## toRefs

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
import {computed, ref} from 'vue';

export default {
	setup() {
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
	// Options API Way
	props: ['firstName', 'lastName']
}
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
  loadingComponent: Spinner
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

Setup

```bash
$ npm install -g @vue/cli
$ vue create project-name
$ winpty vue.cmd create hello-world (type this if step two arrow key not work)
```
