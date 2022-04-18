<template>
  <div class="container">
      <div class="card form-control">
              <div class="form" @click.prevent>
                <form action="">
                  <input type="text" placeholder="User name" v-model.trim="content">
                  <button class="send" @click="sendUser">Send</button>
                  <button class="edit_1">Edit</button>
                </form>
              </div>

      <div class="wrapper">
        <ul>
          <li>
            <span> Username</span>
            <div class="btns">
              <button class="edit">Canel</button>
              <button class="edit"> Edit</button>
              <button class="delete">Delete</button>
            </div>
          </li>
        </ul>
      </div>

      </div>
  </div>
</template>

<script>


export default {
  data () {
    return {
      content: '',
      API : 'https://vue-uzb-login-default-rtdb.firebaseio.com/users.json'
    }
  },
  methods: {
  async  sendUser () {
      if(this.content.length ===0 || this.content.length <= 3) {
        return
      }
      


      // Api  post

      try {
        const res = await fetch(this.API, {
          method : 'POST',
          headers: {
            'Content-type': 'application/json'
          },
          body : JSON.stringify({
            user: this.content
            
          })
        })
        const data  = await res.json()
        console.log(data);
      }
      catch (err) {
        console.log(err);
      }

      this.content = ""
    },

    //  GET
    
  async  getUsers () {
      try{
        const res = await fetch (this.API)
        const data = await res.json()
        console.log(data)
      }
      catch(error) {
        console.log(error)
      }
    }
  }
}
</script>

<style lang="scss">
    .send{
      margin-top: 20px;
      width: 100%;
      background-color: aqua;
      border: 0;
      padding: 10px;
    }
    .edit_1{
      width: 100%;
      margin-top: 20px;
      background-color: coral;
      border: 0;
      padding: 10px;
    }
</style>
