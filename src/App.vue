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
        <ul class="users">
          <li 
          v-for="(user, key) in users"
          :key="key"
          >
            <span> {{ user.user }} </span>
            <div class="btns">
              <button class="edit">Canel</button>
              <button class="edit"> Edit</button>
              <button class="delete" @click="deleteUser(key)">Delete</button>
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
      users: {},
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
        this.users[data.name] = {
          user: this.content
        }
        
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
        this.users = data
        console.log(this.users)
        
      }
      catch(error) {
        console.log(error)
      }
    },

    // DELETE  user 
    async deleteUser (key) {
      try {
        await fetch('https://vue-uzb-login-default-rtdb.firebaseio.com/users/${key}.json',{method: 'DELETE'})
        delete this.users[key]
    
      }
      catch (error) {
        console.log(error)
      }
    }
  },
  mounted () {
    this.getUsers() 
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
