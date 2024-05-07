<template>
  <form>
    <label>Email</label>
    <input type="email" required v-model="email">
    
    <label>Password</label>
    <input type="password" required v-model="password">
    
    <label>Role</label>
    <select v-model="role">
      <option value="developer">Web Developer</option>
      <option value="designer">Web Designer</option>
    </select>

    <label>Skills</label>
    <ul v-if="skills.length">
      <li v-for="skill in skills" key="skills.indexOf(skill)">
        {{ skill }}
        <i class="fa fa-trash" @click="deleteSkill"></i>
      </li>
    </ul>
    <p id="error" v-if="error">{{ error }}</p>
    <input type="text" v-model="tempSkill" @keyup.enter="addSkill">

    <div class="terms">
      <input type="checkbox" required v-model="termsAccepted">
      <label>Accept terms and conditions</label>
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
      error: ''
    }
  },
  methods: {
    addSkill() {
      if (!this.tempSkill) {
        this.error = 'Cannot submit empty values!'
        return
      }
      if (this.skills.length > 0) {
        for (const skill of this.skills) {
          if (skill.toUpperCase() === this.tempSkill.toUpperCase()) {
            this.error = `You've already added that skill!`
            return
          } 
        }
      }
      this.skills.push(this.tempSkill.trim())
      this.tempSkill = ''
      this.error = ''
    },
    deleteSkill(e) {
      const skill = e.target.parentElement.innerText.trim()
      const idx = this.skills.indexOf(skill)
      console.log(idx > -1)
      if (idx > -1) {
        this.skills.splice(idx, 1)
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
  #error {
    color: red;
  }
  i.fa-trash:hover{
    cursor: pointer;
  }
</style>