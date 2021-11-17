# Calculator project

> Project carried out from the modern web course of Professor Leonardo Leitão.
The deployment was done on netfly and can be accessed at:
[projeto calculadora em vue](https://serene-goldberg-92102a.netlify.app/)

## Project setup
```npm install```

## Compiles and hot-reloads for development
```npm run serve```

## Compiles and minifies for production
````npm run build````

## Lints and fixes files
````npm run lint````

## Calculator image

![Calculator](/src/images/calculator.PNG)

## Some codes used

#### main.js

```javascript
import Vue from 'vue'
import App from './App'

new Vue({
    render: h => h(App)    
}).$mount("#app")
```

#### Calculator.vue code

```vue
<template>
  <div class="calculator">
      <Display :value="displayValue" />
      <Button label="AC" triple @meuOnClick="clearMemory" />      
      <Button label="/" operation @meuOnClick="setOperation" />      
      <Button label="7" @meuOnClick="addDigit" />      
      <Button label="8" @meuOnClick="addDigit" />      
      <Button label="9" @meuOnClick="addDigit" />      
      <Button label="*" operation @meuOnClick="setOperation" />      
      <Button label="4" @meuOnClick="addDigit" />      
      <Button label="5" @meuOnClick="addDigit" />      
      <Button label="6" @meuOnClick="addDigit" />      
      <Button label="-" operation @meuOnClick="setOperation" />      
      <Button label="1" @meuOnClick="addDigit" />      
      <Button label="2" @meuOnClick="addDigit" />      
      <Button label="3" @meuOnClick="addDigit" />      
      <Button label="+" operation @meuOnClick="setOperation" />      
      <Button label="0" double @meuOnClick="addDigit" />      
      <Button label="." @meuOnClick="addDigit" />      
      <Button label="=" operation @meuOnClick="setOperation" />    
         
    </div>
</template>

<script>
import Button from "../components/Button"
import Display from "../components/Display"

export default {
    data: function() {
        return {
            displayValue: "0",
            clearDisplay: false,
            operation: null,
            values: [0, 0],
            current: 0
        }
    },
    components: { Button, Display},
    methods: {
        clearMemory() {
            Object.assign(this.$data, this.$options.data())
        },
        setOperation(operation) {
            if (this.current === 0) {
                this.operation = operation
                this.current = 1
                this.clearDisplay = true
            } else {
                const equals = operation === "="
                const currentOperation = this.operation
               
               try {
                   this.values[0] = eval(
                       `${this.values[0]} ${currentOperation} ${this.values[1]}`
                   )
               } catch (e) {
                   this.$emit('onError', e)
               }

               this.values[1] = 0

               this.displayValue = this.values[0]
               this.operation = equals ? null : operation
               this.current = equals ? 0 : 1
               this.clearDisplay = !equals
            }
        },
        addDigit(n) {
            if (n === "." && this.displayValue.includes(".")) {
                return
            }

            const clearDisplay = this.displayValue === "0"
                || this.clearDisplay
            const currentValue = clearDisplay ? "" : this.displayValue
            const displayValue = currentValue + n

            this.displayValue = displayValue
            this.clearDisplay = false
			
            this.values[this.current] = displayValue            
        }
    }
}
</script>

<style>
.calculator {
    height: 321px;
    width: 235px;
    border-radius: 5px;
    overflow: hidden;

    display: grid;
    grid-template-columns: repeat(4, 25%) ;
    grid-template-rows: 1fr 48px 48px 48px 48px 48px;
}
</style>
```