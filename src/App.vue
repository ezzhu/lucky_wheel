<template>
  <div id="app">
    <lucky-wheel></lucky-wheel>
  </div>
</template>

<script>
  import LuckyWheel from './components/lucky_wheel'

  export default {
    name: 'app',
    components: {
      LuckyWheel
    }
  }
</script>

<style>
  body,
  html {
    height: 100%;
  }
  li {
    list-style: none;
  }
  html {
    -ms-text-size-adjust: 100%;
    -webkit-text-size-adjust: 100%;
  }
  body {
    font-size: 16px;
    background: #fff;
  }
  img,input,button,textarea {
    border: 0;
    margin: 0;
    padding: 0;
  }
  a {
    text-decoration: none;
    color: #000000;
    -webkit-tap-highlight-color: rgba(0, 0, 0, 0.1);
  }
  input{
    outline: none;
    -webkit-appearance: none;
  }
  body,h1,h2,ul,li,p{
    margin: 0;
    padding: 0;
  }
  #app{
    height: 100%;
  }
</style>
