<template>
  <div id="app">

    <div id="createNewTask">
      <div class="column">Date Night Ideas</div>
    </div>

    <div>
      Restaurants:
    </div>

  </div>
</template>

<script>


export default {
  name: 'App',
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 100px;
  max-width: 400px;
  margin: 75px auto;
}
#createNewTask {
  background-color: #13283bb6;
  font-size: 22px;
  color: rgba(255, 255, 255, 0.815);
  margin: 1em auto;
  border-radius: 10px;
  height: 75px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
