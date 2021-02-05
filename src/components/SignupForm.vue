<template>
  <form @submit.prevent="handleSubmit"> <!-- remember that submit events have a default action of refreshing the page, so you have to prevent this default action if you want to handle the submit event your own way-->
     <!-- In VueJS, you prevent the default action of a submit event by using an event modifier on the submit event like this: @submit.prevent -->
      <label>Email</label>
      <input type="email" required v-model="email"> <!-- v-model vue directive is used to sync data in a frorm with a variable in our vue component -->
      <!-- v-model directive uses two way binding: that is to say, it binds input from the from to the linked component variable, and also if we change the component variable ourselves, like say inside a method, it also binds it to the input in the form -->

      <label>Password</label>
      <input @keyup="handleSubmit" type="password" required v-model="password">
      <div v-if="passwordError" class="error"> {{ passwordError }}</div>

      <label>Role:</label>
      <select v-model="role">
          <option value="developer">Web Developer</option>
          <option value="designer">Web Designer</option>
      </select>

      <label>Skills:</label>
      <input type="text" v-model="tempSkill" @keyup.alt="addSkill"> <!-- alt + <any key> results in the value of the key not being captured, but the even object associated with the key is captured, but the key's value is not captured anywhere! -->
      <div @click="deleteSkill(skill)" v-for="skill in skills" :key="skill" class="pill"> <!-- always ensure that the key="" attribute required for a v-for(which is not using a computed value), the value of that key must always be unique, if you are cycling through an array, make sure that each element inside the array is unique and an instance of the element of the array as the value of the key. To do this, you will need to data bind the key so the key attribute will become :key="<variable name>" -->
          {{ skill }}
      </div>

      <div class="terms">
          <input type="checkbox" v-model="terms" required>
          <label>Accept terms and conditions</label>
      </div>

      <!-- <div>
          <input type="checkbox" value="Shaun" v-model="names">
          <label>Shaun</label>
      </div>

      <div>
          <input type="checkbox" value="Yoshi" v-model="names">
          <label>Yoshi</label>
      </div>

      <div>
          <input type="checkbox" value="Mario" v-model="names">
          <label>Mario</label>
      </div> -->
    <div class="submit">
        <button>Create an account</button> <!-- a button inside an html form, when clicked, causes the form to have a submit event-->
    </div>
  </form>

  <p>Email: {{ email }}</p>
  <p>Password: {{ password }}</p>
  <p>Role: {{ role }}</p>
  <p>Terms accepted: {{ terms }}</p>
  <!-- <p>Names: {{ names }}</p> -->
</template>

<script>
export default {
    data() {
        return {
            email: "",
            password: "",
            role: "designer",
            terms: false,
            // names: []
            tempSkill: "",
            skills: [],
            passwordError: ""
        }
    },
    methods:{
        addSkill(e){
            if(e.key === "," && this.tempSkill.trim()){
                if(!this.skills.includes(this.tempSkill.trim())){
                    this.skills.push(this.tempSkill.trim())
                }
                this.tempSkill = ""
            }
        },
        deleteSkill(skill){
            this.skills = this.skills.filter((skila) => {
                return !(skill == skila)
            })
        },
        handleSubmit(){
            // validate password
            this.passwordError = this.password.length > 5 ?
             "" : "Password must be at least 6 chars long"
        }
    }
}
</script>

<style>
form{
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
}
label{
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
}
input, select{
    display: block;
    padding: 10px 6px; /* how far the content should be from its borders */
    width: 100%;  /* this sets the width to the width length of its containing parent element*/
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555
}
input[type="checkbox"]{
    display: inline-block;
    width: 16px;
    margin: 0 10px 0 0;
    position: relative; /* this is relative to its normal postion in the document flow */
    top: 2px;
}
.pill{
    display: inline-block;
    margin: 20px 10px 0 0; /* when you are giving margin to ensure spacing between inline-block elements, give only margin-left or only margin-right, don't set both! */
    padding: 6px 12px;
    background: #eee;
    border-radius: 20px;
    font-size: 12px;
    letter-spacing: 1px;
    font-weight: bold;
    color: #777;
    cursor: pointer; /* if you want to indicate that an element is clickable, you can set the cursor to pointer. */
}
button{
    background: #0b6dff;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
}
.submit{
    text-align: center;
}
.error{
    color: #ff0062;
    margin-top: 10px;
    font-size: 0.8em; /* this is relative to its parent's font-size: rem, on the other hand, is relative to the root font-size(which in most browsers is 16px be default) */
    font-weight: bold;
}

</style>