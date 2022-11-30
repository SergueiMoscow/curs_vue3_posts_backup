<template>
  <div class="post">
    <div>
      <div><strong>Название поста:</strong>{{ post.title }}</div>
      <div><strong>Описание:</strong>{{ post.body }}</div>
      <div><strong>Id:</strong>{{ post.id }}</div>
    </div>
    <div class="post__btns">
      <my-button
        @click="$emit('remove', post)"
      >Удалить</my-button>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    post: {
      type: Object,
      required: true
    }
  },
  emits: ['remove'],
  methods: {
    // removePost() {
    //   console.log("PostItem:")
    //   console.log(this.post.id)
    //   console.log(this.post)
    //   this.$emit('remove', this.post)
    // }
  }
}
</script>
<style scoped>
.post {
  padding: 10px 15px;
  border: 2px solid teal;
  margin: 2px;
  margin-top: 10px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>
