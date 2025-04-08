<template>
  <form @submit.prevent="handleSubmit" class="theme-pink" style="width: 500px">
    <label for="email">Email:</label>
    <input type="text" id="email" v-model="email" required />

    <label for="password">Password:</label>
    <input type="password" id="password" v-model="password" required />
    <span v-if="passwordError" class="error">{{ passwordError }}</span>

    <label for="role">Role:</label>
    <select id="role" v-model="role">
      <option value="developer">Web Developer</option>
      <option value="designer">Web Designer</option>
    </select>

    <div class="checkbox-container">
      <input type="checkbox" id="terms" v-model="terms" required />
      <label for="terms" class="inline-label"
        >Accept Terms and Conditions</label
      >
    </div>

    <label for="skills">Skills:</label>
    <input
      type="text"
      id="skills"
      v-model="tempSkill"
      @keyup.enter="addSkill"
      @keyup.comma="addSkill"
      placeholder="Type a skill and press Enter or comma"
    />
    <div class="skills-container">
      <div
        v-for="(skill, index) in skills"
        :key="index"
        class="pill"
        @click="removeSkill(index)"
      >
        {{ skill }} <span class="remove-icon">×</span>
      </div>
    </div>

    <button type="submit">Submit</button>
  </form>

  <div class="preview">
    <h3>Preview:</h3>
    <p><strong>Email:</strong> {{ email }}</p>
    <p><strong>Password:</strong> {{ password }}</p>
    <p><strong>Role:</strong> {{ role }}</p>
    <p><strong>Terms accepted:</strong> {{ terms }}</p>
    <p><strong>Skills:</strong> {{ skills.join(", ") || "None added yet" }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: "",
      password: "",
      role: "developer",
      terms: false,
      tempSkill: "",
      skills: [],
      passwordError: "", // Add this line
    };
  },
  methods: {
    addSkill(e) {
      if (
        (e.key === "Enter" || e.key === "," || e.type === "keyup") &&
        this.tempSkill.trim()
      ) {
        let skill = this.tempSkill.replace(/,/g, "").trim();
        if (!this.skills.includes(skill) && skill) {
          this.skills.push(skill);
        }
        this.tempSkill = "";
      }
    },
    removeSkill(index) {
      this.skills.splice(index, 1);
    },
    handleSubmit() {
      this.passwordError =
        this.password.length >= 8
          ? ""
          : "Password must be at least 8 characters long.";

      if (!this.passwordError) {
        console.log("Form submitted!", {
          email: this.email,
          password: this.password,
          role: this.role,
          terms: this.terms,
          skills: this.skills,
        });
      }
    },
  },
};
</script>

<style>
body {
  margin: 0;
  background: #f5f5f5;
  font-family: Arial, sans-serif;
  color: #333;
}

form {
  background-color: #fff;
  padding: 40px;
  border-radius: 18px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
}

.theme-pink {
  background-color: #ffa8d8;
}

label {
  display: block;
  margin: 25px 0 10px;
  font-size: 14px;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: 600;
  color: #555;
}

.theme-pink label {
  color: #fff;
}

.inline-label {
  display: inline-block;
  margin: 0;
  text-transform: none;
  letter-spacing: normal;
  position: relative;
  top: -2px;
}

.checkbox-container {
  margin: 25px 0;
}

input,
select,
textarea {
  display: block;
  width: 100%;
  padding: 12px;
  box-sizing: border-box;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 16px;
  transition: border-color 0.3s, box-shadow 0.3s;
}

input:focus,
select:focus,
textarea:focus {
  outline: none;
  border-color: #ff8ac6;
  box-shadow: 0 0 0 3px rgba(255, 168, 216, 0.2);
}

input[type="checkbox"] {
  display: inline-block;
  width: 16px;
  height: 16px;
  margin: 0 10px 0 0;
  position: relative;
  top: 2px;
  cursor: pointer;
}

button {
  background-color: rgb(252, 253, 249);
  color: rgb(253, 138, 195);
  width: 100%;
  border: 2px solid #ffffff;
  padding: 12px 20px;
  margin-top: 30px;
  border-radius: 6px;
  cursor: pointer;
  font-weight: bold;
  font-size: 16px;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #ff69b4;
}

.skills-container {
  margin-top: 15px;
  display: flex;
  flex-wrap: wrap;
}

.pill {
  display: inline-flex;
  align-items: center;
  margin: 5px 10px 5px 0;
  padding: 6px 12px;
  background: #fff;
  border-radius: 20px;
  font-size: 12px;
  letter-spacing: 1px;
  font-weight: bold;
  color: #ff69b4;
  cursor: pointer;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  transition: background-color 0.2s;
}

.pill:hover {
  background-color: #ffecf5;
}

.remove-icon {
  margin-left: 6px;
  font-size: 14px;
}

.preview {
  max-width: 420px;
  margin: 20px auto;
  padding: 20px;
  background-color: #fff;
  border-radius: 12px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
}

.preview h3 {
  margin-top: 0;
  color: #ff69b4;
  border-bottom: 2px solid #ffecf5;
  padding-bottom: 10px;
}

.error {
  color: #e74c3c;
  font-size: 12px;
  margin-top: 5px;
}
</style>
