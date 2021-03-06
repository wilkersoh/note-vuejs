  * [LifeCycle](#lifecycle)
  * [Reactivity system](#reactivity-system)
  * [Standardise Tool use in VueJs](#standardise-tool-use-in-vuejs)
  * [SFC (Single File Component)](#sfc--single-file-component-)
  * [directive hook](#directive-hook)
  * [v-directive](#v-directive)
    + [v-model](#v-model)
    + [v-model.[modifier] - trim, number, lazy, capitalize](#v-model-modifier----trim--number--lazy--capitalize)
    + [custom modifier](#custom-modifier)
    + [v-bind](#v-bind)
    + [v-bind without argument](#v-bind-without-argument)
    + [v-on without argument](#v-on-without-argument)
    + [complex exmaple (v-bind & v-on)](#complex-exmaple--v-bind---v-on-)
    + [@click.[modifier] (v-on:)](#-click-modifier---v-on--)
    + [@change (trigger when user leaves the input)](#-change--trigger-when-user-leaves-the-input-)
    + [v-on:input - @input (onChange)](#v-on-input----input--onchange-)
    + [@keydown.enter](#-keydownenter)
    + [v-pre](#v-pre)
  * [data() return Object](#data---return-object)
  * [Loop](#loop)
  * [$attrs && inheritAttrs](#-attrs----inheritattrs)
  * [ref](#ref)
  * [Reactive](#reactive)
  * [toRefs and reactive](#torefs-and-reactive)
  * [slot (children)](#slot--children-)
  * [v-slot](#v-slot)
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
    + [beforeEach (scroll to top)](#beforeeach--scroll-to-top-)
  * [Vue delimiters](#vue-delimiters)
  * [Transition Component](#transition-component)
  * [nextTick](#nexttick)
  * [forceRerender](#forcererender)
  * [markRaw](#markraw)
  * [Vee-validate (Composition API)](#vee-validate--composition-api-)
    + [schema](#schema)
    + [Best practice for yup](#best-practice-for-yup)
    + [With Reactive](#with-reactive)
    + [Note](#note)
    + [handle multiple input errors (useForm)](#handle-multiple-input-errors--useform-)
    + [error show input name](#error-show-input-name)
    + [dirty](#dirty)
    + [initialValues && initialErrors](#initialvalues----initialerrors)
    + [useForm handle submit](#useform-handle-submit)
    + [non-AJAX submition](#non-ajax-submition)
    + [Submit as  nested object](#submit-as--nested-object)
  * [Field ??? Form | :validation-schema](#field---form----validation-schema)
    + [Field](#field)
    + [Form & validaton](#form---validaton)
    + [Change modalValue default name](#change-modalvalue-default-name)
  * [Component Design Patterns](#component-design-patterns)
    + [Props custom validation](#props-custom-validation)
    + [scoped slots (pass value to parent from child)](#scoped-slots--pass-value-to-parent-from-child-)
  * [render function with component](#render-function-with-component)
  * [Functional Components](#functional-components)


## LifeCycle

   `LifeCycle methods`       `after lifeCycle step what vue to do`

1. beforeCreate             ??? then Observe data & initialize events
2. created                      ??? then Compile template (created?????????init???data???props???data??????
3. beforeMount             ??? then Create vm.$el & update DOM
4. mounted                    ??? then `Mounted`?????? ????????? Data Changed
5. beforeUpdate            ??? Re-render virtual DOM & patch
6. updated                      ??? `Mounted`

    `Destroy LifeCycle`

                                            ??? when vm.$destroy() is called

1. beforeDestroy        ??? remove watchers, components and event listeners.

                                       `Destroyed`

 2.  destroyed

## Reactivity system

Vue?????????system????????????????????? ??????state????????? ?????????re-render??????component

1. Vue?????????????????? ?????? `Object.defineProperty` ????????? `gettter/setter`
2. ?????????????????? ????????? Vue ???????????????????????????????????????????????? getter ??? setter????????????dependency??????(dependency-track)???????????????(change-notification
3. ?????? data() { return { name: ''??? } } ?????? Vue `initialise`??????`??????` `step2`???????????????????????????+?????? data ?????? initialise ?????????????????? ?????? ????????????reactivity???(???????????????vue???)
4. ????????????????????????????????? watcher ??????????????????????????????????????????????????????????????????`dependency`????????????`dependency`?????? setter ???????????????????????? watcher ????????????????????????????????????????????????????????????(trigger setter to notify the watcher)

## Standardise Tool use in VueJs

1. Vue Router 
2. Vuex - state management 
3. Vue test utils
4. Vue devtools - debuging 
5. vee-validate & yup
6. vue-multiselect && Vue-autosuggest 

## SFC (Single File Component)

```html
<template></template>

<script></script>

<style></style>
```

## directive hook

1. **bind**????? called once when the directive is bound to an element
2. **inserted**????? when the bound element is inserted into its parent node
3. **update**????? when the element updates (but any children haven???t yet)
4. **componentUpdated**????? after the children have also updated
5. **unbind**????? called once when the directive is unbound from an element

## v-directive

1. v-model
2. v-bind:href="url" - `:href`
3. v-for
4. v-on:click - `@click`
5. @dblclick=??????
6. @sumbit.prevent="fnName"
7. v-if (????????? ????????????insert??????dom ???????????????????????????state)
8. v-show (dom ??????????????? ?????? css display control?????????????????????)
9. v-else (???????????? v-if ??????)
10. @mouseover=???methodName??? method??????????????? (e) ??????e. ????????????pass???????????? ?????????define
11. @mouseleave=???methodName($event, 5)??? ?????????pass args ????????? ???event ???????????? method?????????event?????????

### v-model

1. mainly for input and form binding, so use it when you dealing with various input types
2. v-model is for two way bindings means: if you change input value, the bound data will be changed and vice versa. (???????????????data ????????????origin data???????????????)

### v-model.[modifier] - trim, number, lazy, capitalize

```html
<input type="text" v-model.trim="formValues.name" \>

// ????????????update ???????????????input???update ?????? ?????? ??????
<input type="text" v-model.trim.lazy="formValues.name" \>

<input type="text" v-model.lazy="value" />

// ?????? ??? number
<input type="text" v-model.number="value" />

```

### custom modifier

```jsx
// parent
// add capitalize
<SalutationInput v-model:salutation.capitalize="value" />
// vue3 will inject a new props for custom modifier, eg: below
v-model:salutation // salutationModifiers { Object }
v-model:name // nameModifiers { Object }

// SalutationInput.vue
<script>
	props: {
		salutationModifiers: {
			type: Object,
			default: () => ({})
		}
	},
	setup() {
		const updateSalutation = (event) => {
      let val = event.target.value;
      if(props.salutationModifiers.capitalize) {
        val = val.toUpperCase();
      }

      emit("update:salutation", val)
    }
	}
</script>

<template>
	<select @change="updateSalutation" name="salutation">
		<option
			loopDataItem
			:value="item"
			:selected="salutation === item"
		>...</option>
	</select>
</template>
```

### v-bind

1. allow you to produce some dynamic value by typing some JS expression that in most cases depends on the data from data model - so think about v-bind as directive that you should use when you want deal with some dynamic things
2. v-bind:value is called one way binding that means: you can change input value by changing bound data but you can't change bound data by changing input value through the element(??????data ?????????????????????data ?????????????????? ????????????data, ???????????? `bind`??? `html attribute, custom props`)

### v-bind without argument

```jsx
export default {
  return {
		imageAttrs: {
			src: 'better',
			alt: 'code'
		}
	}
}

<img v-bind:src="imageAttrs.src" v-bind:alt="imageAttrs.alt" />

// clean way
<img v-bind="{ src:imageAttrs.src, alt:imageAttrs.alt }" />

// better way
<img v-bind="imageAttrs" />
```

### v-on without argument

```jsx
export default {
	const openGallery = () => { ... }
	const showTooltip = () => { ... }
	return {
		imageEvents: { openGallery, showTooltip } // ????????? ??????3
	}
}

<img v-on:click="openGallery" v-on:mouseover="showTooltip" />

<img v-on="{ click: openGallery, on:mouseover: showTooltip}" />

// ??????3
<img v-on="imageEvents" />
```

### complex exmaple (v-bind & v-on)

```jsx
<template>
	<Component
		v-for="content in apoResponse"
		:key="content"
		:is="content.type"
		v-bind="feedItem(content).attrs"
		v-on="feedItem(content).events"
	/>
</template>
// ?????? ?????? Component :is ????????? ??? v-bind ??? v-on 
return {
	feedItem(item) {
		if(item.type === 'xx') { return { attrs: xxx, events: xxxx }}
		else if(item.type === 'yy') { return { attrs: yy, events: yyyy }}
	}
}
```

### @click.[modifier] (v-on:)

> v-on directive is how you run JavaScript in response to DOM events. If you want to run some code when the user clicks a button, you should use v-on

```html
<template>
	<div class="overlay" @click.self="closeModal">
		<div class="modal">
			<p>Some modal content...</p>
			<p>?????? overlay??? parent ?????? click.self???????????? ?????????????????????layout???trigger
				closeModal???????????? ??????children modal ?????????trigger?????????function???
				?????????bubbling??? dom???
			</p>
		</div>
	</div>
</template>
```

### @change (trigger when user leaves the input)

```jsx
<input @change="callthisValidateFn" v-model="value" />
```

### v-on:input - @input (onChange)

```jsx
<input
   :value="something"
   @input="something = $event.target.value"
>
```

### @keydown.enter

```jsx
<input @keydown.enter="clickEnterCallFn" />
```

### v-pre

```html
<div v-pre>{{renderAsStringNotParseToVue}}</div>
```

## data() return Object

```jsx
export default {
	data() {
		return { 
				items: [],
				formValues: { name: '', age: ''}
		 }	
	},
	// ???????????????
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

## $attrs && inheritAttrs

```jsx
// parent
/*
	By default it will pass down to the first div
	child component ???????????? set props??? ????????????pass
	?????? ???????????????????????????div??????
	????????? attrs
*/
<template>
	<Child :color="bgColor" id="12" /> 
</template>

// Child
<template>
	<div>
		<div v-bind=???$attrs??? :class="$attrs.bgColor"></div>
	</div>
</template>

export default {
	inheritAttrs: false, // ?????? pass ??? attrs ??? div ?????????root??? div ???+
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
// ??? Composition API??? ?????? ref value ?????? ????????? reactivity in vue system
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
	???????????? Primitive in reactive
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
			 ??? Template ?????? {{ posts }} ????????? ????????? state.posts??? ?????? toRefs?????????
			 ????????? return ??? state } 
			 template ?????? state.posts?????????????????????
			 ????????????????????? state. ?????????
			 return { firstName: state.firstName, }
			 ???????????? ?????? posts ??? update ?????? view ?????? vue????????? 
			 state??????????????????value?????????
			 ???????????? return toRefs(state) 
			 Template ???????????? ???????????? state. ?????? posts
		*/
		...toRefs(state), 
		load,
	}
}
```

## slot (children)

```html
// parent, v-slot ????????? template tag
<Modal>
	<template v-slot:nameSlot>
		<p>????????????child ?????????????????? vue???directive??????????????????</p>
  </template>
	<p>some text</p>
<Modal>

// Modal Component
<template>
	<div>
		// ????????? p tag children ??????????????? slot
		<slot></slot>
		<div>
			// ?????????slot ??? ?????? Modal child??????template???????????????
			<slot name="nameSlot"></slot>
    </div>
	</div>
</template>

```

```jsx
// parent
// string
<template v:slot:header></template>
// js variable
<template v:slot:[slotName]></template>
```

## v-slot

```jsx
// app.vue
<template>
  <current-user v-slot="{user}">
    {{ user.firstName }}
  </current-user>
</template>

// v-slot has alias also
v-slot:header="data" > #header="data"
we can specifc #header instead of v-slot:header
```

```jsx
// my-modal.vue children
<template>
<div>
	<div>
		<slot name="header"></slot>
		<button>Close</button>
	</div>
	<div>
		<slot name="body"></slot>
	</div>
</div>
</template>

// other .vue parent
<template>
	<my-modal>
		<template #header><h2>This will be put to the local</h2></template>
		<template v-slot:body><h2># is a alias from v-slot</h2></template>
	</my-modal>
</template>
```

Pass value to parent from `<slot>`

```jsx
<!--BookCard.vue--> children
<template>
	....
	<slot name="preview" :previewSlug="previewSlug"></slot>
</template>

<!--BookList.vue--> parent
<template>
	<book-card>
		...
		<template v-slot:preview="slotProps">
			<a :href="slotProps.previewSlug" />
		</template>
	</book-card>
</template>

```

Pass data to children from parent

```jsx
<!--BookList.vue--> parent
<template>
	... loop
	<book-card :book-data="book">
		...
		<template v-slot:preview="slotProps">
			<a :href="slotProps.previewSlug" />
		</template>
	</book-card>
</template>

<!--BookCard.vue--> parent
export default {
	props: {
		bookData: {
      // we can use camelCase instead of kebab even top is assign kebab
    }
	}
}
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
// Composition API ?????? method ????????????+??? 
import {computed, ref, reactive} from 'vue';

export default {
	props: ['propsName'],
	setup(props) {
		/*
			1. ?????????ref ??? bind data
			2. ????????? this ??? setup???
		*/
		const { a, b } = toRefs(props) // destructure 

		// data
		// mounted
		// methods
		// lifecycle hooks
  }
}
```

1.  ??????project?????????mantain
2. ??????code ?????? share???

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

```jsx
// child - component name (ChildComponent)
<template>
	<div @click="$emit('clickedChild', $event)" :data-index="value"></div>
</template>

export default {
	emits: ["clickedChild"],
}

// parent
<template>
	<ChildComponent @clickedChild="clickedChild" />
</template>

export default {
	const clickedChild = (e) => {
		console.log(e)
	}
}
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

// Suspense ?????? fallback ??? User component ??? aysnc ????????? return??????

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

`computed` return ????????? variable ?????? ???????????? ???`????????????` computed ?????? ?????? variableName.value??? computed ?????????????? `ref` 

```jsx
// ??? UI re-render ??????call ??? getTotal??????computed ??????dependency ????????? ?????????re-render???

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
			// fullName ??? ?????? computed??????object
			this.fullName = "laoyeche Wilker" // ?????? value ??? set???(value)
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

```jsx

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
			?????????????????? ??????default??? 
			???????????????pass??? ??????dependency???????????? ?????????????????????newValue
		*/
		volume(newValue, oldValue) { // ??????volume????????? data?????? volume key??????
			if(newValue > oldValye && newValue === 16) {
				console.log("high volume will hurt your hearing")
			}
		},
		movie: { // ?????? ????????? obj, ?????? obj ????????????????????????????????? immediate???key
			handler(newValue) {
				console.log('handler name is provided from vue')
			},
			immediate: true, // ????????? mounted ??????call handler function
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
			deep: true, // ???????????? true??? ?????? concat array?????????????????????array??? 
			//????????? push??? ????????? modified ?????????????????? deep??? true
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
			options: { // array or object ?????? deep true
				heroName: '',
			},
		});
	
		//  ?????? watch lName or fName
		watch(() => {
			// ???????????? return state ???copy ????????? object 
			// ?????? oldValue ?????? newValue ????????????
			return { ...state } // ?????? return ????????????????????? newValue value
		}, (newValue, oldValue) => {
			console.log(newValue.fName)
		});

  	// watch ????????????
	  watch(() => state.fName, (newValue, oldValue) => {
	  	console.log("if watch state independent value need callback the state value")
  		console.log("Doesn't need newValue.fName since cb state.fName", newValue)
  	})

		// watch ?????? state
		watch(state, newValue => {
			//....
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
}sl
```

you pass an array as a first argument (to watch multiple sources), that array can contain mix of functions and naked refs. See example bellow.

??????????????? ????????? ?????? ?????? trigger watch???

Vue cannot detect property addition or deletion. Since Vue performs the getter/setter conversion process during instance initialization.  Vue does not allow dynamically adding new root-level reactive properties to an already created instance. That is NOT reactive if you `change directly,` so watch cannot detected.

```jsx
const app = Vue.createApp({  
  setup() {
    const state = Vue.reactive({
      postPerPage: 10,
      currentPage: 1,
    });

    const posts = Vue.ref([]);
    
    const normalRef = Vue.ref(0)
    
    const watchCounter = Vue.ref(0)

    Vue.watch([
        //() => posts.value, // <-- this will not trigger watch callback on array push (only on array replace). Can be fixed by deep watch (uncomment last argument of watch)
        //posts, // <-- same result as above
        () => [ ...posts.value],         
        // () => state, // similar to array, this does not trigger on property change
        () => ({ ...state }), 
        normalRef
      ],
      (newValues, prevValues) => {
        watchCounter.value++
        console.log(`prev (${watchCounter.value}):`, prevValues);
        console.log(`new (${watchCounter.value}):`, newValues);
      },
      // { deep: true }
    )
    
    return {
      state,
      posts,
      normalRef,
      watchCounter
    }
  },
  mounted() {
    const interval = 1000
    setTimeout( () => this.state.currentPage = 2, interval)
    setTimeout( () => this.posts.push({ a: 1 }), interval*2)    
    setTimeout( () => this.normalRef = 10, interval*3)    
  }
})

app.mount("#app")
```

```jsx
export default {
	setup() {
		const counter = reactive({count: 1});
		watch(() => counter.count, () => {})
		
		const count = ref(0);
		watch(count, () => {})

		const count2 = ref(1);
		watch([count, count2], () => {});

		watch([
			count, 
			() => ({ ...counter })
		], () => {})

	}
}
```

## Provide/Inject

Vue re-renders only the affected descendent components and not all the components along the component hierarchy

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

```jsx
// Composition Api (parent)
export default {
	const settings = ref({
		catsApiKey: 'e8d29058-baa0-4fbd-b5c2-3fa67b13b0d8',
    catsSearchApi: 'https://api.thecatapi.com/v1/images/search',
	});
	provide('settings', computed(() => settings.value)); // (name, value)
	const changeSearchApi = (searchApi) => {
        settings.value = {
            ...settings.value,
            catsSearchApi: searchApi
        };
    };

	return { changeSearchApi }
}

// children
export default {
	const settings = inject('settings', {}); // (theNameOfPropertyInject, optionalOfDefaultValue) 

	const loadNextImage = async () => {
    try {
        const { catsApiKey, catsSearchApi } = settings.value;
        
        axios.defaults.headers.common['x-api-key'] = catsApiKey;
        
        let response = await axios.get(catsSearchApi, {params: { limit: 1, size: "full" } });
        const { url } = response.data[0];
        imageUrl.value = url;
	    } catch (err) {
        console.log(err)
    }
  };
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
		// ????????? ?????? access reactive data and event???
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

export default {
	setup() {
		onMounted(async () => {
			loading.value(true);
			data.value = await fetch("url");
			loading.value(false)
		})

		return { loading, data }
	}
}
```

## Mixins

```jsx
/*
	Click ??? Hover ?????????????????? 
	?????????Mixins 
	- ?????? ?????? ????????? data() ?????? return ??? counter.js ??? variable
		?????? ?????? component ??? overwrite???
*/
// ClickCounter.vue
// Note: ????????? ??????????????? incrementCount and count ???????????? ?????? app??????
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

### beforeEach (scroll to top)

```jsx
router.beforeEach((to, from, next) => {
  window.scrollTo(0, 0)
  // ...
})
```

## Vue delimiters

```jsx
// Default: ["{{", "}}"] ?????? ????????? value ??? ??? {{ username }}
// ???????????????

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

????????????????????? ??? `enter-to`,  `leave-from` ??? css ?????? ????????? ???????????? animation ??? default css??? ?????? opacity 1 or scale 1???

```css
/* Default */
.enter-from { opacity: 0 };
.enter-to { opacity: 1 }; /* default ?????? ??????????????? */
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
	mode: by default ????????????????????????
	(out-in | in-out)
	??????????????? ?????? ??? mode??????????????? ?????? ?????? list item??????????????? v-else???
	?????? ?????? ?????? ????????? list-leave-from ??? transition ???????????????
	?????? v-else ??????????????????
	?????? ??? out-in ????????? ???????????? leave??? ??????in??? 
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
EnterHook: ???life cycle > before-enter ??????
before-enter, enter, after-enter

LeavesHook: ???life cycle???
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
			y: 0, // translateY ??????
			duration: 3, // 3 sec
			onCompleted: done, // after duration finish call done if not 
							           // afterEnter will trigger 
			delay: el.dataset.index * 0.2
		});
	 }
		

	}
</script>
```

## nextTick

?????? ??????data??????updated???DOM????????????callback???????????????

Imagine that you need to perform some action when a component is mounted. BUT! not only the component. You also need to wait until all its children are mounted and available in the DOM. Damn! Our mounted hook doesn???t guarantee that the whole component tree renders

```jsx
mounted() {
  this.$nextTick(() => {
    // The whole view is rendered, so I can safely access or query
    // the DOM. ??\_(???)_/??
  })
}
```

## forceRerender

## markRaw

??? v2??? ?????? ?????? yup.object({}), vue ???????????? yup?????? reactive object

????????? `markRaw`  ?????????????????????

```jsx
import { markRaw } from 'vue';

{
  data() {
    // Non-reactive because it was explicitly defined with `markRaw`
    const schema = markRaw(yup.object({
      email: yup.string().required().email(),
      password: yup.string().required().min(8),
    }));

    return {
      schema,
    };
  },
}
```

## Vee-validate (Composition API)

```jsx
<template>
  <div>
    <input v-model="value" type="text" />
    <span>{{ errorMessage }}</span>
  </div>
</template>
```

`useField` ?????????????????? set rules

```jsx
import { useField } from 'vee-validate';
const isRequired = (value) => { return value === true || "some error msg"}
const { errorMessage, value } = useField('fieldName', isRequired);
```

use `yup`

```jsx
import { useField } from 'vee-validate';
import * as yup from 'yup';
const { errorMessage, value } = useField('fieldName', yup.string().required().min(8));

```

### schema

??? `useForm` ?????? validate??? schema

```jsx
const simpleSchema = {
  email(value) {
    // validate email value and return messages...
  },
  name(value) {
    // validate name value and return messages...
  },
};

useForm({
  validationSchema: simpleSchema,
});

const { value: email, errorMessage: emailError } = useField('email');

<template>
	<input name="email" v-model="email" />
</template>
```

### Best practice for yup

Should import what you need to control the bundler's tree-shaking

```jsx
import { object, string } as yup from 'yup';

const schema = object({
  email: string().email(),
  // ...
});
```

??? `yup` ?????? schema

```jsx
import { useForm, useField } from 'vee-validate';
import * as yup from 'yup';

const schema = yup.object({
  email: yup.string().required().email(),
  name: yup.string().required(),
  password: yup.string().required().min(8),
});

const { handleSubmit, errors } = useForm({
  validationSchema: schema,
});

const { value: email, errorMessage: emailError } = useField('email');
```

### With Reactive

```jsx
import { computed, ref } from 'vue';
import { useForm, useField } from 'vee-validate';
import * as yup from 'yup';

const min = ref(0);
const schema = computed(() => {
  return yup.object({
    password: yup.string().min(min.value), // ????????? value
  });
});

// Create a form context with the validation schema
useForm({
  validationSchema: schema,
});

const { value: password, errorMessage: passwordError } = useField('password');
```

handleChnage from useField

```jsx
const { errorMessage, value, handleChange } = useField('fieldName', isRequired);

/* Lister every input */
<template>
	<input @input="handleChange" :value="value" type="text" />
</template>

/* Lister after leave the input */
	<input @change="handleChange" :value="value" type="text" />
```

### Note

????????? lazy mode ??? aggressive mode

1. lazy mode - ??? ????????? ??? ?????? validate
2. aggressive mode - ??? listen ?????? input change

```jsx
/* ???????????? ???user ??????input??? validate ?????? error
	user ??????????????? ?????? lazy mode ????????? aggressive mode
	??? input ??? invalid ???valid ?????? ??? ????????? lazy mode
*/
<template>
  <div>
    <input v-on="validationListeners" :value="value" type="text" />
    <span>{{ errorMessage }}</span>
  </div>
</template>

import { computed } from 'vue';
import { useField } from 'vee-validate';

export default {
  setup() {
    function isRequired(value) {
      // ...
    }

    const { errorMessage, value, handleChange, handleInput } = useField('fieldName', isRequired, {
      validateOnValueUpdate: false,
    });

    const validationListeners = computed(() => {
      // If the field is valid or have not been validated yet
      // lazy
      if (!errorMessage.value) {
        return {
          blur: handleChange,
          change: handleChange,
          input: handleInput,
        };
      }

      // Aggressive
      return {
        blur: handleChange,
        change: handleChange,
        input: handleChange, // only switched this
      };
    });

    return {
      errorMessage,
      value,
      validationListeners,
    };
  },
};
</script>
```

### handle multiple input errors (useForm)

```jsx
const schema = yup.object({
    email: yup.string().required().email(),
    password: yup.string().required().min(8),
});

// Create a form context with the validation schema
const { errors } = useForm({
  validationSchema: schema,
});

const { value: email } = useField('email');
const { value: password } = useField('password');
```

### error show input name

```jsx
// default
error show: The down_p is required

// Custom with yup
const schema = Yup.object({
  email_addr: Yup.string().email().required().label('Email Address'),
  acc_pazzword: Yup.string().min(5).required().label('Your Password'),
});

```

### dirty

`useField` or `useForm` 

```jsx
<template>
  <input v-model="value" type="text" />

  <button :disabled="!meta.dirty">Submit</button>
</template>

import { useField, useForm } from 'vee-validate';

// Provide initial value to make `meta.dirty` accurate
const { value, meta } = useField('fieldName', undefined, {
  initialValue: '',
});

```

????????? ?????????`<form>` tag, 

### initialValues && initialErrors

```jsx
/**
	???????????????????????? initialValue
???????????? useForm
**/
<template>
  <form @submit="submit">
    <input v-model="value" type="text" />

    <button :disabled="!meta.dirty">Submit</button>
  </form>
</template>

const { meta, values } = useForm({
  initialValues: {
    email: '',
  },
	initialErrors: {
		email: 'This email is already taken'
	},
});

// create some fields with useField
const { value } = useField('email');

function submit() {
  // send stuff to api
}
```

### useForm handle submit

`handleSubmit` , `isSubmitting`, `resetForm`, `setFieldError`, `setErrors`

```jsx
<template>
	<form @submit="onSubmit">
			/*  form fields */
		<button :disabled="isSubmitting" type="submit"></button>
	</form>
</template>

const { handleSubmit, isSubmitting, resetForm } = useForm();

const onSubmit = handleSubmit(values => {
	// send to api

	resetForm();
});

return {
  onSubmit,
};
```

### non-AJAX submition

You normally would use this if you are not building a single-page application.

```jsx
<template>
  <form action="/users" method="post" @submit="submitForm">
    <!-- some fields -->
  </form>
</template>

const { submitForm } = useForm();

return {
  submitForm,
};
```

### Submit as  nested object

```jsx
<template>
  <form @submit="onSubmit">
    <input v-model="twitter" type="url" />
    <input v-model="github" type="url" />

    <button>Submit</button>
  </form>
</template>

export default {
  setup() {
    const { handleSubmit } = useForm();
    const onSubmit = handleSubmit(values => {
      alert(JSON.stringify(values, null, 2));
/*
			{
			  "links": {
			    "twitter": "https://twitter.com/logaretm",
			    "github": "https://github.com/logaretm"
			  }
			}
*/
    });

    const { value: twitter } = useField('links.twitter');
    const { value: github } = useField('links.github');
		/*
			Nested Array
	    const { value: twitter } = useField('links[0]');
	    const { value: github } = useField('links[1]');
		*/
    return {
      twitter,
      github,
      onSubmit,
    };
  },
};
```

## Field ??? Form | :validation-schema

### Field

??????????????? `Field` ??????????????? ??????custom???rules???

```jsx
<Field name="password" type="password" rules="required|min:8" />

/* ????????? setup??? */
import { defineRule } from 'vee-validate';

defineRule('minLength', (value, [limit]) => { // limit??? min:8 ??? 8??????
  // The field is empty so it should pass
  if (!value || !value.length) {
    return true;
  }

  if (value.length < limit) {
    return `This field must be at least ${limit} characters`;
  }

  return true;
});
```

### Form & validaton

```jsx
<template>
  <Form @submit="submit" :validation-schema="schema" v-slot="{ errors }">
    <Field name="email" />
    <span>{{ errors.email }}</span>

    <Field name="password" type="password" />
    <span>{{ errors.password }}</span>

    <button>Submit</button>
  </Form>
</template>

<script>
import { Form, Field } from 'vee-validate';

export default {
  components: {
    Form,
    Field,
  },
  data() {
    const schema = {
      email: 'required|email',
      password: 'required|min:8',
    };

    return {
      schema,
    };
  },
};
</script>
```

Vue3 add typescript to existing project

```bash
$ vue add typescript
use class-styled - N
Use Babal... - Y
Convert all .js to .ts - Y
Allow .js files to be compiled - N
...recomanded of app - Y
```

v-model in Vue2 and Vue3

```tsx
// v2
<template>
	<input
		:value="value"
		@input="$emit(
			'input',
			$event.target.value
		)"
	/>
</template>

export default {
  props: {
		value: {
			type: [String, Number],
			default: '',
		}
	}
}
```

```jsx
// v3
<template>
	<input
		:value="modalValue"
		@input="$emit(
			'update:modelValue',
			$event.target.value
		)"
	/>
</template>

export default {
  props: {
		modelValue: {
			type: [String, Number],
			default: '',
		}
	}
}

```

### Change modalValue default name

```jsx
// v3 it can change the modalValue
<template>
	<BaseInput 
		v-model:modalValue="someValue"
	/>
</template>

export default {
  props: {
		modelValue: {
			type: [String, Number],
			default: '',
		}
	}

```

## Component Design Patterns

### Props custom validation

```jsx
export default {
	props: {
		image: { 
			/*
				?????? 
				1. ???????????? png and jpg
				2. path ??? images/
			*/
			type: String,
			default: '/images/placeholder.png',
			validator: propValue => {
				const hasImagesDirectory = propValue.indexOf('/images/') > -1;
				const isPNG = propValue.endsWith('.png');
				const isJPG = propValue.endsWith('.jpg');
				const hasValidateExtension = isPNG || isJPG;
				return hasImagesDirectory && hasValidateExtension;
			}
		}
	}
}
```

### scoped slots (pass value to parent from child)

```jsx
<slot name="header" :logo="logoImage" />

// parent
<template v-slot:header="slotProps">
	{{ slotProps.logo }}
</template>

// clean code
<template v-slot:header="{ logo }">
	{{ logo }}
</template>
```

Example

```jsx
// Book.vue children 
<template>
	<div>
		<slot name="title"
				:bookTitle="bookTitle"
		/>
	</div>
</template>

export default {
	setup() {
		return {
			bookTitle: 'hi'
		}
	}
}

// Library.vue parent
<template>
	<Book>
		<template v-slot:title="slotPorps">
			<h1>{{ slotPorps.bookTitle }}</h1>
		</template>
	</Book>
</template>
```

## render function with component

```jsx
<div id="app"></div>

<script>
	const GreetComponent = {
		template: '<h2>HEllo</h2>'
	}
	let app = new Vue({
		el: '#app',
		render(h) {
			return h(GreetComponent)
		}
	})
</script>
```

## Functional Components

1. Can't have their own data, computed properties, watchers, lifecycle method or methods
2. Can't have a template, unless it's precompiled from a single-file component 
3. Can be passed things, like props, events, attributes and slots.
4. Return a Vnode or an array of VNodes from render function

FC ???????????????  

[https://medium.com/js-dojo/vue-js-functional-components-what-why-and-when-439cfaa08713#:~:text=What is a functional component,itself through the this keyword](https://medium.com/js-dojo/vue-js-functional-components-what-why-and-when-439cfaa08713#:~:text=What%20is%20a%20functional%20component,itself%20through%20the%20this%20keyword) 

```jsx
<script>
export default {
	functional: true,
	props: { SomeProps: String },
	render(h, ctx) {
		// functional must be true and use render function
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
