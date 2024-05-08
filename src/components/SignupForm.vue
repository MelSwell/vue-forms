<template>
  <form @submit.prevent="handleSubmit">
    <label>Email</label>
    <input type="email" required v-model="email">
    
    <label>Password</label>
    <p class="error" v-if="passwordError">{{ passwordError }}</p>
    <input type="password" required v-model="password">
    
    <label>Role</label>
    <select v-model="role">
      <option value="developer">Web Developer</option>
      <option value="designer">Web Designer</option>
    </select>

    <label>Skills</label>
    <p class="error" v-if="skillsError">{{ skillsError }}</p>
    <input type="text" v-model="tempSkill" @keydown.tab="addSkill">
    <br>
    <ul class="skills" v-if="skills.length">
      <li v-for="skill in skills" :key="skill">
        {{ skill }}
        <i class="fa fa-trash" @click="deleteSkill"></i>
      </li>
    </ul>

    <div class="terms">
      <input type="checkbox" required v-model="termsAccepted">
      <label>Accept terms and conditions</label>
    </div>

    <div class="submit">
      <button>Create Account</button>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
      email: '',
      password: '',
      role: '',
      termsAccepted: false,
      tempSkill: '',
      skills: [],
      skillsError: '',
      passwordError: ''
    }
  },
  methods: {
    addSkill() {
      if (!this.tempSkill) {
        this.skillsError = 'Cannot submit empty values!'
        return
      }
      if (this.skills.length > 0) {
        for (const skill of this.skills) {
          if (skill.toUpperCase() === this.tempSkill.toUpperCase()) {
            this.skillsError = `You've already added that skill!`
            return
          } 
        }
      }
      this.skills.push(this.tempSkill.toUpperCase().trim())
      this.tempSkill = ''
      this.skillsError = ''
    },
    deleteSkill(e) {
      const skill = e.target.parentElement.innerText.toUpperCase().trim()
      const idx = this.skills.indexOf(skill)
      if (idx > -1) {
        this.skills.splice(idx, 1)
      }
    },
    handleSubmit() {
      this.passwordError = this.password.length > 5 ? 
        '' : 'Password must at least 6 characters long'
        
      if (!this.passwordError && !this.skillsError) {
        // POST to server
        console.log('email: ', this.email)
        console.log('pasword: ', this.password)
        console.log('role: ', this.role)
        console.log('terms: ', this.termsAccepted)
        console.log('skills: ', this.skills)
      }
    }
  }
}
</script>

<style>
  form {
    background: white;
    max-width: 420px;
    margin: 30px auto;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
  }
  label {
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6 em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
  }
  input, select {
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555
  }
  input[type="checkbox"] {
    display: inline-block;
    width: 16px;
    margin: 0 10px 0 0;
    position: relative;
    top: 2px; 
  }
  p.error {
    color: red;
    font-size: 12px;
    letter-spacing: 1px;
    font-weight: bold;
  }
  i.fa-trash:hover {
    cursor: pointer;
  }
  ul.skills {
    margin-top: 0px;
    text-transform: uppercase;
    font-size: .9em;
    font-weight: bold;
    display: flex;
    flex-direction: column;
    padding-left: 20px;
  }
  ul.skills li {
    padding: 6px 12px;
    font-size: 12px;
    letter-spacing: 1px;
    font-weight: bold;
    color: #777;
    list-style-type: none;
    background-color: #eee;
    border-radius: 20px;
    width: fit-content;
    margin-bottom: 5px;
    align-self: flex-start;
  }
  .terms label{
    margin-top: 10px;
  }
  button {
    background: #06bdff;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
  }
  .submit {
    text-align: center;
  }
</style>