<template>
  <nav-bar></nav-bar>
<div class = "app">
  <router-view></router-view>
</div>
</template>
<script>
import NavBar from '@/components/NavBar'
export default {
  components: {
    NavBar
  }
}
</script>
<style>
body {
  padding: 0px;
  margin: 0px;
  box-sizing: border-box;
}
.app {
  padding: 15px;
}
.app__btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}
/* .page__wrapper {
  display: flex;
  margin-top: 15px;
}
.page {
  border: 1px solid black;
  padding: 10px;
  margin: 5px;
  cursor: pointer;
}
.current-page {
  border: 3px solid teal;
} */
.observer {
  height: 30px;
  background: gray;
}
</style>
