<!-- src/components/InputQuestion.vue -->
<template>
  <form @submit.prevent="handleSubmit" class="input-question">
    <dl>
      <dt>{{message}}</dt>
      <dd>
        <input v-model="name" type="text" required>
      </dd>
    </dl>
    <button>次へ</button>
  </form>
</template>

<script>

  export default {
    data () {
      return {
        name: ' ',
      }
    },
    props: {
      message: String,
    },
    methods: {
      handleSubmit () {
        this.$emit('input', this.name)
        this.$emit('next')
      },
    },
  }
</script>

<style scoped>

  h3 {
    font-size: 1em;
  }

  dd {
    margin: 16px 0 0;
    padding: 0 0 8px 0;
  }

  button {
    width: 60px;
  }

  input[type='text'] {
    margin-right: 20px;
  }
</style>
