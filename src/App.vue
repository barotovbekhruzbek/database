<template>
  <div class="container">
      <div class="card form-control">
              <div class="form" @click.prevent>
                <form action="">
                  <input type="text" placeholder="User name" v-model.trim="content">
                  <button class="send" @click="sendUser" v-if="!isEditing">Send</button>
                  <button class="edit_1" v-if="isEditing">Edit</button>
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
              <button class="edit" v-if="isEditing && key === activeKey" >Canel</button>
              <button class="edit" @click="beforeEdit(key, user.user)"> Edit</button>
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
      isEditing: true,
      activeKey : null,
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
        await fetch(`https://vue-uzb-login-default-rtdb.firebaseio.com/users/${key}.json`,{method: 'DELETE'})
        delete this.users[key]
    
      }
      catch (error) {
        console.log(error)
      }
    },
    beforeEdit(key,text) {
      this.isEditing = false
      this.content = text
      this.activeKey = key  
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
    .delete{
      padding: 10px;
      background-color: brown;
      border: 0;
      color: #fff;
      border-radius: 6px;
      margin-left: 10px;
    }
    .edit{
      padding: 10px;
      background-color: sandybrown;
      border: 0;
      color: #fff;
      border-radius: 6px;
      margin-left: 10px;
    }
</style>
