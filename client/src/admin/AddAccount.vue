<template> 
  <div>
    <h1>Add Account</h1>
    <div class="register">
      <label for="lblogin">Login Name</label> 
      <input type="text" v-model="model.account.login_name" placeholder="Enter Login Name" id="lblogin"  class="form-control"/>
      <label for="lbdisplay">Display Name</label>     
      <input type="text" v-model="model.account.display_name" placeholder="Enter Display Name" id="lbdisplay" class="form-control" />
      <label for="lbpassword">Password</label>
      <input type="password" v-model="model.account.pass_word" placeholder="Enter Password" id="lbpassword" class="form-control" />
      <div class="example ex1">
        <h5>Select Role</h5>
        <label class="radio red">
          <input type="radio" v-model="model.account.role_id" value="1" name="group1" />
          <span>Admin</span>
        </label>
        <label class="radio blue">
          <input type="radio" v-model="model.account.role_id" value="2" name="group1" />
          <span>User</span>
        </label>
      </div>
      <button v-on:click="addAccount">Add Account</button>
    <!--  <p>
        <router-link to="/">Home</router-link>
      </p> -->
    </div>
  </div>
</template>

<script>
//import BackEndService from "../BackEndService";
import axios from "axios";

export default {
  name: "AddAccount",
  data() {
    return {
      errlst: "",
      model: {
        account: {
          login_name: "",
          display_name: "",
          pass_word: "",
          role_id: "",
        }
      }

    };
  },
  methods: {
    async addAccount() {
      try{
      const url = "http://localhost:3000/api/v1/database/account/add";
      alert("url" + url + " login: " + this.model.account.login_name + " displayname: " + this.model.account.display_name + " role_id: " + this.model.account.role_id);
      let result = await axios.post(url, {
        login_name: this.model.account.login_name,
        display_name: this.model.account.display_name,
        pass_word: this.model.account.pass_word,
        role_id: this.model.account.role_id,
      });
      console.warn(result);
      if (result.status == 201) {
        alert(result.data);
        localStorage.setItem("user-info", JSON.stringify(result.data))
        this.$router.push({ name: 'admin' })
      }
    }
    catch(err) {
      alert(err)
        this.errlst = err.errors
    }
    },
  },
};
</script>

<style scoped>
.example {
  margin: 20px;
}

.example input {
  display: none;
}

.example label {
  margin-right: 20px;
  display: inline-block;
  cursor: pointer;
}

.ex1 span {
  display: block;
  padding: 5px 10px 5px 25px;
  border: 2px solid #ddd;
  border-radius: 5px;
  position: relative;
  transition: all 0.25s linear;
}

.ex1 span:before {
  content: '';
  position: absolute;
  left: 5px;
  top: 50%;
  -webkit-transform: translatey(-50%);
  transform: translatey(-50%);
  width: 15px;
  height: 15px;
  border-radius: 50%;
  background-color: #ddd;
  transition: all 0.25s linear;
}

.ex1 input:checked+span {
  background-color: #fff;
  box-shadow: 0 0 5px 2px rgba(0, 0, 0, 0.1);
}

.ex1 .red input:checked+span {
  color: red;
  border-color: red;
}

.ex1 .red input:checked+span:before {
  background-color: red;
}

.ex1 .blue input:checked+span {
  color: blue;
  border-color: blue;
}

.ex1 .blue input:checked+span:before {
  background-color: blue;
}

.ex1 .orange input:checked+span {
  color: orange;
  border-color: orange;
}

.ex1 .orange input:checked+span:before {
  background-color: orange;
}</style>