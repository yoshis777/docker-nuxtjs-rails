<template>
  <section>
    <div>
      <h1>New user</h1>
      <form @submit.prevent="post">
        <label for="name">Name: </label>
        <input id="name" v-model="name" type="text" name="name" />
        <button type="submit">submit</button>
      </form>
    </div>
  </section>
</template>

<script>
    export default {
        data() {
            return {
                name: ''
            }
        },
        methods: {
            post() {
                this.$axios.post(
                    '/users',
                    {
                        name: this.name
                    }
                ).then((res) => {
                    this.$router.push(`${res.data.id}`)
                })
            }
        }
    }
</script>
