<template>
  <form @submit.prevent="handleSubmit" novalidate>
    <label>Email:</label>
    <div v-if="error.emailError" class="error">{{ error.emailError }}</div>
    <input type="email" required v-model="email" placeholder="example@mail.com" @blur="checkEmail" tabindex="1">
    <label>Password:</label>
    <div class="pass-block">

      <input :type="passType" required v-model="password" placeholder="do not use 123 or equal"
             @blur="toggleShowPassReq"
             class="password" @focus="toggleShowPassReq" tabindex="2">
      <span :class="passCheckbox ? 'check' : 'uncheck'">
      <input type="checkbox" required v-model="passCheckbox" class="pass-type"></span>
    </div>
    <div class="passreq" v-if="showPassReq">
      <ul>
        <li :class="passReqLength ? 'pass-done' : ''">password must be at least 6 chars long</li>
        <li :class="passReqContain ? 'pass-done' : ''">password must contain numbers and letters</li>
      </ul>
    </div>
    <label>repeat password:</label>
    <div v-if="error.passError" class="error">{{ error.passError }}</div>
    <div class="pass-block">

      <input :type="passType" required v-model="passwordRep" placeholder="passwords must be the same"
             class="password" tabindex="3" style="width: 100%">
    </div>
    <label>Role:</label>
    <div vif="roleError" class="error">{{ error.roleError }}</div>
    <select v-model="role" @blur="checkRole" tabindex="4" class="select-block">
      <option value="developer">Frontend Developer</option>
      <option value="developer">Backend Developer</option>
      <option value="designer">Web Designer</option>
      <option value="lead">Team Lead</option>
    </select>

    <div>
      <label>enter your skills:</label>
      <div v-if="error.skillError" class="error">{{ error.skillError }}</div>
      <input type="text" @keydown.space="addSkill" v-model="inputSkillValue" placeholder="press space for each one"
             tabindex="5">
    </div>
    <div class="pill" v-for="(skill, idx) in skills" @click="deleteSkill(idx)">
      {{ skill }}
    </div>
    <div class="terms">
      <span :class="agreement ? 'checkTerm' : 'uncheckTerm'">
        <input type="checkbox" required v-model="agreement" class="term" tabindex="6"></span>
      <label class="term-label">Accept terms and conditions</label>
    </div>
    <div class="submit">
      <button @click.self="realSubmit">Create an Account</button>
    </div>
  </form>

</template>

<script>
export default {
  name: 'SignupForm',
  props: {},
  data() {
    return {
      email: '',
      password: '',
      passwordRep: '',
      passType: 'password',
      passCheckbox: false,
      passReqLength: false,
      passReqContain: false,
      showPassReq: false,
      role: '',
      agreement: true,
      inputSkillValue: '',
      skills: ['html', 'css', 'JavaScript', 'Vue3.js'],
      error: {
        passError: '',
        roleError: '',
        skillError: '',
        emailError: ''
      }
    }
  },
  methods: {
    addSkill() {
      if (!this.skills.includes(this.inputSkillValue) && this.inputSkillValue) this.skills.push(this.inputSkillValue)
      this.inputSkillValue = ''
    },
    deleteSkill(idx) {
      this.skills.splice(idx, 1)
    },
    handleSubmit() {
      this.checkRole()
      console.log(this.error)
      if (!this.error.roleError && !this.error.emailError && !this.error.passError && !this.error.skillError && !this.passReqLength && !this.passReqContain) return false
      alert('submit')
    },
    checkPass() {
      this.error.passError = this.password !== this.passwordRep ? 'passwords do not match' : ''
    },
    checkEmail() {
      this.error.emailError = !(this.email.includes('@') && this.email.includes('.')) ? 'please, enter correct email' : ''
    },
    checkRole() {
      this.error.roleError = this.role === '' ? 'please, choose your role' : ''
    },
    checkSkills() {
      this.error.skillError = this.skills.length === 0 ? 'please, enter some skills' : ''
    },
    toggleShowPassReq() {
      this.showPassReq = (!(this.passReqLength && this.passReqContain))
    }
  },
  watch: {
    passCheckbox() {
      this.passType = this.passCheckbox ? 'text' : 'password'
    },
    password() {
      this.passReqLength = this.password.length > 5
      this.passReqContain = (/\d/.test(this.password) && /[a-z, A-Z]/.test(this.password))
    },
    passwordRep() {
      this.checkPass()
    },
    role() {
      this.checkRole()
    },
    email() {
      this.checkEmail()
    },
    skills() {
      this.checkSkills()
      console.log('s')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
</style>
