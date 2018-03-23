# fmadvvuejsfeafromgroundup

## Reactivity
### Challenge 1: Getters and Setters
```
const obj= {foo:123}
convert(obj);
obj.foo // getting key "foo":'123'
obj.foo=234 //seting key "foo" tp '234'
```
### Challenge 1: Solution
```
function convert(obj){
  Object.keys(obj).forEach(key=> {
    let internalValue = obj[key]
    Object.defineProperty(obj,key,{
      get(){
        console.log(`getting key "${key}":${internalValue}`)
        return internalValue
      },
      set(newValue){
        console.log(`setting key "${key}" to: ${newValue}`)
        internalValue = newValue
      }
    })
  })
}
```


### Dependency tracking
Goal
- create a `Dep` class with two methods, `depend` and `notify`
- create an `autorun` function that takes an updater function
- Inside the updater function you can explicitly depend on an instance of `Dep` by calling `dep.depend()`
- Later, you can trigger the updater function to run again by calling `dep.notify()`
```
window.Dep = class Dep{
  constructor(){
    this.subscribers=new Set()
  }
  depend(){
    if(activeUpdate){
      this.subscribers/add(activeUpdate)
    }
  }
  
  notify(){
    this.subscribers.forEach(sub=>sub())
  }
}
let activeUpdate
function autorun(update){
  function wrappedUpdate(){
    activeUpdate = wrappedUpdate;
    update()
    activeUpdate=null
  }
}

autorun(()=>{
  dep.depend()
})
```



## Writing plugins
```
const vm = new Vue({
  data: {foo:10},
  rules:{
    foo:{
      validate: value=>value>1,
      message: 'foo must greater than 1'
    }
  }
})
vm.foo=0 //should log `foo must greater than 1`
```

solution
```
Vue.use(myPlugin)
const myPlugin = {
  install(Vue){
    Vue.mixin({
      created(){
        if(this.$options.rules){
          Object.keys(this.$options.rules).forEach(key=>
          {const rule=this.$options.rules[key]
            this.$watch(key,(newValue)=>{
              const = rule.validate(newValue)
              if(!result){
                console.log(rule.message)
              }
            })
          }
          ))
        }
       }
    })
  }
}
```



## Render functions

### Virtual DOM
```
document.createElement('div')
vm.$createElement('div')
```
virtual dom def:
- A lightweight javascript data format to represent what the actual DOM should look like at a given point in time
- Decouples rendering logic from the actual dom - enables rendering capabilities in non-browser environments
- eg server-side and native mobule rendering


### Challenge 5: Dynamically Rendering Tags
implement example component
```
<example :tags=['h1','h2']></example>
```
renders
```
<div>
  <h1>0</h1>
  <h2>1</h2>
</div>
```

### Challenge 5: Solution
