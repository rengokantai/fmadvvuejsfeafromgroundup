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
