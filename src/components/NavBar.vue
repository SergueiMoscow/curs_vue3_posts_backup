<template>
    <div class="nav-bar">
        <div @click="$router.push('/')">Vue 3</div>
        <div class="navbar__btns">
            <my-button @click="$router.push('/posts')">Посты</my-button>
            <my-button style="margin-left: 20px;" @click="$router.push('/about')">О приложении</my-button>
        </div>
    </div>
</template>
<script>
import MyButton from '@/components/UI/MyButton'
export default {
  components: { MyButton },

}
</script>
<style scoped>
.nav-bar {
    height: 50px;
    background: lightgray;
    box-shadow: 2px 2px 4px gray;
    display: flex;
    align-items: center;
    padding: 0 15px;
}
.navbar__btns {
    margin-left: auto;
}
</style>
