<template>
  <div id="app">
      <Constructor></Constructor>
  </div>
</template>

<script>
    import Constructor from './components/Constructor'

    export default {
        components:{
            Constructor
        },
        name: 'App'
    }
</script>

<style>
    html, body{
        margin:0;
        height:100%;
        font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
        background-color: #f5f5f5;
        color: #4D4D57;

    }

    #app{
        height:100%;
    }

</style>
