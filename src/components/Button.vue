<script setup>
import { defineProps } from 'vue'
const props = defineProps({
  text: String,
})
</script>

<template>
  <div>
    <button>{{props.text}}</button>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Oswald:700&display=swap');

div{
  padding-bottom: 40px;
}

button {
  background: hsl(0, 100%, 73%);
  color: white;
  padding: 0.2em 1.5em;
  border: none;
  border-radius: 10em;
  font-size: 2em;
  text-transform: uppercase;
  letter-spacing: .15em;
  outline: none;
  font-weight: 900;
  cursor: pointer;
  transition: all .5s cubic-bezier(0.215, 0.61, 0.355, 1);;
  font-family: 'Oswald', sans-serif;
  text-shadow:  0 1px hsl(0, 0%, 80%),
  0 2px hsl(0, 0%, 78%),
  0 3px hsl(0, 0%, 75%),
  0 3px 5px hsl(0, 0%, 0%, .4),
  0 4px 20px hsl(0, 0%, 0%, .35);
  box-shadow: 0 1px hsl(0, 100%, 75%),
  0 2px hsl(0, 100%, 69%),
  0 3px hsl(0, 100%, 68%),
  0 4px hsl(0, 100%, 68%),
  0 5px hsl(0, 100%, 68%),
  0 6px hsl(0, 100%, 68%),
  0 7px hsl(0, 100%, 68%),
  0 8px hsl(0, 100%, 68%),
  0 9px hsl(0, 100%, 68%),
  0 10px hsl(0, 100%, 68%),
  0 11px hsl(0, 100%, 68%),
  0 12px hsl(0, 100%, 68%),
  0 13px hsl(0, 100%, 68%),
  0 14px hsl(0, 100%, 68%),
  0 15px hsl(0, 100%, 68%),
  0 16px hsl(0, 100%, 68%),
  0 17px hsl(0, 100%, 68%),
  0 18px hsl(0, 100%, 68%),
  0 19px hsl(0, 100%, 68%),
  0 20px hsl(0, 100%, 68%),
  0 21px hsl(0, 100%, 68%),
  0 22px hsl(0, 100%, 68%),
  0 23px hsl(0, 100%, 68%),

  0 23px 10px hsla(0, 0%, 0%, .8),
  0 25px 50px hsla(0, 0%, 0%, .5)
}

button:hover {
  background: hsl(0, 100%, 72%);
  box-shadow: none;
  text-shadow: none;
  transform: translateY(25px);
}
</style>