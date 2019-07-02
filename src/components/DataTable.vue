<template>    
  <div> 
    <table class="table">
      <thead>
        <tr>
          <th class="no-header">No.</th>
          <th class="name-header">Name</th>
          <th class="phone-header">Phone</th>
          <th class="email-header">Email</th>
          <th class="action-header">Action</th>
        </tr>
      </thead>
      <tbody>
        <template v-for="user in users">
          <tr v-if="user.active" :key="user.id">
            <template v-for="(property, key) in user">
              <td v-if="key !== 'active'" :key="user.id + key" :class="getAlignment(key)">
                <template v-if="!isEditable(key)">
                  {{ property }}
                </template>   
                <template v-else>
                  <Input :type="key" />
                </template>       
              </td>
            </template>
            <td>
              <button class="action-button" @click="toggleEdit(user.id)">Edit</button>
              <button @click="removeRow(user.id)">Remove</button>
            </td>
          </tr>
        </template>
      </tbody>      
    </table>  

    <Modal v-if="showModal" @close="showModal = false">
      <template slot="body">
        <form @submit="saveChanges" class="edit-form">
          <label class="input-label">Name: <input type="text" v-model="trackEdit.name" value="test"></label>
          <label class="input-label">Phone: 
            <input type="tel" v-model="trackEdit.phone" pattern="[0-9]{2}-[0-9]{7}">
          </label>
          <label class="input-label">Email: <input type="email" v-model="trackEdit.email"></label>          
          
          <div class="submit-wrapper">
            <button type="submit" class="submit-button">Save changes</button>      
          </div>              
        </form>                       
      </template>
    </Modal> 
  </div>   
</template>

<script>
import Input from './Input'
import Modal from './Modal'

export default {
  name: 'DataTable',
  components: {
    Input,
    Modal
  },
  data() {
    return {
      users: [
        { id: 1, name: 'David', phone: '02-1234567', email: 'david@mail.com', active: true },
        { id: 2, name: 'Anna', phone: '02-3528151', email: 'anna@mail.com', active: true },
        { id: 3, name: 'Doris', phone: '02-8791587', email: 'doris@mail.com', active: true },
        { id: 4, name: 'Peter', phone: '02-7777777', email: 'peter@mail.com', active: true },
        { id: 5, name: 'Mom', phone: '02-9999999', email: 'mom@mail.com', active: true },
        { id: 6, name: 'Dad', phone: '02-7654123', email: 'dad@mail.com', active: true }
      ],
      showModal: false,      
      trackEdit: {
        id: '',
        name: '',
        phone: '',
        email: ''        
      }      
    }
  },
  methods: {
    getAlignment(key) {
      if (key === 'id') {
        return { 'align-td-center': true }
      } 
      else {
        return { 'align-td-left': true }
      }     
    },
    isEditable(key) {
      // let editable = ['name', 'phone', 'email']
      // if (editable.includes(key)) {
      //   return true
      // }
      // else {
      //   return false
      // }

      return false
    },
    toggleEdit(id) {
      // set values in tracker to data of user that is currently being edited
      let currentUser = {}
      for (let user of this.users) {
        console.log(user.id, id)
        if (user.id === id) {
          currentUser = user
        }
      }

      this.trackEdit = {...currentUser}      

      // this.trackEdit.id = id
      this.showModal = true
    },
    saveChanges(e) {
      e.preventDefault()
      
      // get index of user to update
      let userIndex = null
      for (let i = 0; i < this.users.length; i++) {
        if (this.trackEdit.id === this.users[i].id) {
          userIndex = i
        } 
      }

      // if user found, update user
      if (userIndex !== null) {
        this.users[userIndex] = this.trackEdit
      }
      
      // close edit modal
      this.showModal = false   
    },
    removeRow(id) {   // doesn't actually delete user data but instead sets to inactive     
      // get index of user to update
      let userIndex = null
      for (let i = 0; i < this.users.length; i++) {
        if (id === this.users[i].id) {
          userIndex = i
        } 
      }

      console.log(userIndex)

      // if user found, set user to inactive
      if (userIndex !== null) {
        this.$set(this.users, userIndex, false)     
        console.log(this.users)  
      }
    }      
  }  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .table {
    margin: 100px auto;
    width: 800px;    
    border-collapse: collapse;
    border-spacing: 0px;
    
  }

  table, th, td
  {
    padding: 5px;
    border: 1px tomato solid;
  }

  .no-header {
    width: 5%;
    text-align: center;
  }

  .action-header {
    width: 16%;
  }

  .align-td-center {
    text-align: center;
  }

  .align-td-left {
    text-align: left;
  }

  .action-button {
    margin-left: 3px;
    margin-right: 3px;
  }
  
  .input-label {
    margin-left: 5px;
  }

  .submit-wrapper {
    display: flex;
    justify-content: flex-end;
  }

  .submit-button {
    display: block;
    margin-top: 15px;
    margin-right: 25px;
  }

  .edit-form {
    margin-top: 50px;
    height: 100px;
  }
</style>
