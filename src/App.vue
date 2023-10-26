<template>
  <body>
  <header class="py-2">
    <nav class="navbar navbar-expand-lg navbar-light">
      <div class="container-fluid">
        <a class="navbar-brand" href="./">DesignStudio</a>
        <button class="navbar-toggler text-bg-light" type="button" data-toggle="collapse" data-target="#navbarNav"
                aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li class="nav-item">
              <a class="nav-link text-white" href="./">Авторизація</a>
            </li>
            <li class="nav-item">
              <a class="current nav-link" href="./">Реєстрація</a>
            </li>
            <li class="nav-item">
              <a class="nav-link text-white" href="./">FAQ</a>
            </li>
            <li class="nav-item">
              <a class="nav-link text-white" href="./">Контакти</a>
            </li>
            <li class="nav-item">
              <a class="nav-link text-white" href="./">Таблиця</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </header>
  <div class="registration">
    <h1>Реєстрація</h1>
    <form method="post" @submit.prevent="registerUser" class="formData">
      <label for="firstName">Ім’я:</label>
      <input v-model="formData.firstName" type="text" id="firstName" name="firstName" @input="checkFirstName" required>
      <div id="firstName_error" class="invalid">{{ errorMessages.firstName }}</div>
      <br>
      <label for="lastName">Прізвище:</label>
      <input v-model="formData.lastName" type="text" id="lastName" name="lastName" required @input="checkLastName">
      <div id="lastName_error" class="invalid">{{ errorMessages.lastName }}</div>
      <br>
      <label for="middleName">По батькові:</label>
      <input v-model="formData.middleName" type="text" id="middleName" name="middleName" required
             @input="checkMiddleName">
      <div id="middleName_error" class="invalid">{{ errorMessages.middleName }}</div>
      <br>
      <div class="radio">
        <label>Гендер:</label>
        <input type="radio" id="male" v-model="formData.gender" name="gender" value="male" required>
        <label for="male">Чоловік</label>
        <input type="radio" id="female" v-model="formData.gender" name="gender" value="female" required>
        <label for="female">Жінка</label>
      </div>
      <label for="email">Email:</label>
      <input v-model="formData.email" type="email" id="email" name="email" required @input="checkEmail">
      <div id="email_error" class="invalid">{{ errorMessages.email }}</div>
      <br>
      <label for="password">Пароль:</label>
      <input v-model="formData.password" type="password" id="password" name="password" required @input="checkPassword">
      <div id="password_error" class="invalid">{{ errorMessages.password }}</div>
      <br>
      <label for="phone">Телефон</label>
      <input v-model="formData.phone" v-mask="'+38 (0##) ###-##-##'" name="phone" id="phone" placeholder="Телефон"
             required>
      <br>
      <label for="date">Дата народження</label>
      <input v-model="formData.date" type="date" id="date" name="date" max="2023-10-27" required @input="checkDate">
      <div id="date_error" class="invalid">{{ errorMessages.date }}</div>
      <br>
      <label for="group">Група:</label>
      <select id="group" v-model="formData.group" name="group" required>
        <option value="group1">Група 1</option>
        <option value="group2">Група 2</option>
        <option value="group3">Група 3</option>
      </select>
      <br>
      <label for="file">Завантажити файл:</label>
      <input class="input-file-btn" type="file" id="file" name="file">
      <br>
      <button class="sub-btn" type="submit">Зареєструватися</button>
    </form>
  </div>
  <table id="userDataTable">
    <thead>
    <tr>
      <th></th>
      <th>Ім’я</th>
      <th>Прізвище</th>
      <th>По батькові</th>
      <th>Гендер</th>
      <th>Email</th>
      <th>Телефон</th>
      <th>Дата народження</th>
      <th>Група</th>
    </tr>
    </thead>
    <tbody>
    <tr v-for="(u, index) in users" :key="index">
      <td><input type="checkbox" v-model="selectedRows[index]"></td>
      <td>{{ u.firstName }}</td>
      <td>{{ u.lastName }}</td>
      <td>{{ u.middleName }}</td>
      <td>{{ u.gender }}</td>
      <td>{{ u.email }}</td>
      <td>{{ u.phone }}</td>
      <td>{{ u.date }}</td>
      <td>{{ u.group }}</td>
    </tr>
    </tbody>
    <div class="button-container">
      <button class="button" @click="deleteSelectedRows">Видалити вибрані</button>
      <button class="button" @click="duplicateSelectedRows">Дублювати вибрані</button>
    </div>
  </table>
  <footer class="text-white text-center py-3 footer-custom">
    <div class="contact-links">
      <a href="tel:+123456789"><i class="fas fa-phone"></i>Зателефонувати</a>
      <a href="mailto:studio@example.com"><i class="fa-solid fa-envelope"></i>Написати email</a>
      <a href="https://t.me/studio"><i class="fab fa-telegram"></i> Телеграм</a>
      <a href="https://wa.me/studio"><i class="fab fa-whatsapp"></i> WhatsApp</a>
    </div>
  </footer>
  </body>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        firstName: '',
        lastName: '',
        middleName: '',
        email: '',
        password: '',
        phone: '',
        date: '',
        gender: '',
        group: ''
      },
      errorMessages: {
        firstName: '',
        lastName: '',
        middleName: '',
        email: '',
        password: '',
        date: ''
      },
      users: [],
      selectedRows: []
    };
  },
  methods: {
    registerUser() {
      this.users.push({...this.formData});

      this.formData = {
        firstName: '',
        lastName: '',
        middleName: '',
        email: '',
        password: '',
        phone: '',
        date: '',
        gender: '',
        group: 'group1'
      };
    },
    deleteSelectedRows() {
      for (let i = this.selectedRows.length - 1; i >= 0; i--) {
        if (this.selectedRows[i]) {
          this.users.splice(i, 1);
        }
      }
      this.selectedRows = [];
    },
    duplicateSelectedRows() {
      for (let i = this.selectedRows.length - 1; i >= 0; i--) {
        if (this.selectedRows[i]) {
          this.users.splice(i + 1, 0, {...this.users[i]});
        }
      }
      this.selectedRows = [];
    },
    checkFirstName() {
      const firstName = this.formData.firstName;
      this.errorMessages.firstName = this.validateName(firstName);
    },
    validateName(firstName) {
      let error = "";
      if (!firstName.trim()) {
        error += "Поле ім'я не може бути порожнім";
      }
      if (firstName.length > 15) {
        error += "Має бути до 15 символів. ";
      }
      if (/\d/.test(firstName)) {
        error += "Не можна вводити цифри. ";
      }
      if (firstName.charAt(0).toUpperCase() !== firstName.charAt(0)) {
        error += "Перша літера має бути великою. ";
      }
      return error;
    },
    checkLastName() {
      const lastName = this.formData.lastName;
      this.errorMessages.lastName = this.validateLastName(lastName);
    },
    validateLastName(lastName) {
      let error = "";
      if (!lastName.trim()) {
        error += "Поле ім'я не може бути порожнім";
      }
      if (lastName.length > 15) {
        error += "Має бути до 15 символів. ";
      }
      if (/\d/.test(lastName)) {
        error += "Не можна вводити цифри. ";
      }
      if (lastName.charAt(0).toUpperCase() !== lastName.charAt(0)) {
        error += "Перша літера має бути великою. ";
      }
      return error;
    },
    checkMiddleName() {
      const middleName = this.formData.middleName;
      this.errorMessages.middleName = this.validateMiddleName(middleName);
    },
    validateMiddleName(middleName) {
      let error = "";
      if (!middleName.trim()) {
        error += "Поле ім'я не може бути порожнім";
      }
      if (middleName.length > 15) {
        error += "Має бути до 15 символів. ";
      }
      if (/\d/.test(middleName)) {
        error += "Не можна вводити цифри. ";
      }
      if (middleName.charAt(0).toUpperCase() !== middleName.charAt(0)) {
        error += "Перша літера має бути великою. ";
      }
      return error;
    },
    checkEmail() {
      const email = this.formData.email;
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

      if (!emailRegex.test(email)) {
        this.errorMessages.email = "Неправильний формат електронної пошти";
      } else {
        this.errorMessages.email = "";
      }
    },
    checkPassword() {
      const password = this.formData.password;
      this.errorMessages.password = this.validatePassword(password);
    },
    validatePassword(password) {
      let error = '';
      if (/\s+/.test(password)) {
        error += "В паролі не має бути пробілів.  ";
      }
      if (password.length < 8 || password.length > 15) {
        error += "Пароль має бути від 8 до 15 символів.  ";
      }
      if (!/[A-Z]+/.test(password)) {
        error += "В паролі має бути хоча б одна велика латинська літера.  ";
      }
      if (!/[a-z]+/.test(password)) {
        error += "В паролі має бути хоча б одна маленька латинська літера.  ";
      }
      if (/[\u0400-\u04FF]/.test(password)) {
        error += "Тільки латинські літери  ";
      }
      return error;
    },
    checkDate() {
      const date = new Date(this.date);
      if (isNaN(date.getTime()) || date > new Date()) {
        this.date += 'Введіть коректну дату народження.';
      } else {
        this.date += '';
      }
    },
  },
};

</script>

<style>
body {
  font-family: 'Georgia', serif;
}

header {
  background-color: #333;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  margin-bottom: 40px;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

.button-container {
  text-align: center;
}

.button {
  margin-top: 10px;
  background-color: #333;
  color: #fff;
  padding: 10px 20px;
  cursor: pointer;
  border: none;
  border-radius: 4px;
}

.button:hover {
  background-color: #555;
}

.invalid {
  color: red;
  font-family: "Book Antiqua", serif;
}

.radio {
  display: flex;
  gap: 20px;
  margin-bottom: 10px;
}

input[type="radio"] {
  margin-right: 5px;
}

label[for="male"],
label[for="female"] {
  display: flex;
  align-items: center;
  cursor: pointer;
}

input[type="radio"]:checked + label {
  font-weight: bold;
}

.registration {
  text-align: center;
  padding: 20px;
  margin: 10px auto;
  max-width: 400px;
  animation: fadeInLeft 1s ease;
}

@keyframes fadeInLeft {
  from {
    opacity: 0;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.registration img {
  width: 100%;
  max-width: 150px;
  border-radius: 50%;
  margin-bottom: 5px;
}

h1 {
  margin-bottom: 10px;
}

form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

label {
  margin: 1px 0;
  color: #000;
}

input {
  padding: 8px;
  margin-bottom: 8px;
  border: 1px solid #333;
  background-color: #fff;
  color: #000;
  border-radius: 4px;
  width: 100%;
}

select,
input {
  padding: 8px;
  margin-bottom: 8px;
  border: 1px solid #333;
  background-color: #fff;
  color: #000;
  border-radius: 4px;
  width: 100%;
}

input[type="file"] {
  cursor: pointer;
  position: relative;
  overflow: hidden;
}

button.sub-btn {
  background-color: #000;
  color: #fff;
  padding: 10px 20px;
  cursor: pointer;
  border: none;
  border-radius: 4px;
}

button.sub-btn:hover {
  background-color: #333;
  color: #fff;
}

header .navbar-brand {
  color: #fff;
  transition: color 0.3s ease;
  font-size: 60px;
  font-family: 'Brush Script MT', cursive;
}

header .navbar-brand:hover {
  color: rgb(173 216 230);
}


header .navbar-nav .nav-item .current {
  color: dodgerblue;
}

.footer-custom {
  position: relative;
  bottom: 0;
  width: 100%;
  background-color: #333;
  color: #fff;
}

.footer-custom {
  padding-bottom: 60px;
}

.footer-custom a {
  color: #fff;
  text-decoration: none;
  transition: color 0.3s ease;
}

.footer-custom a:hover {
  color: #ddd;
}

.contact-links a {
  color: #fff;
  text-decoration: none;
  margin: 0 10px;
}

.contact-links i {
  margin-right: 5px;
  color: #fff;
}

.contact-links a {
  margin-right: 10px;
  color: #fff;
}

@media (max-width: 767px) {
  .contact-links a {
    display: block;
    margin-bottom: 10px;
  }

  .footer-custom {
    margin-top: 50px;
    bottom: 0;
    position: relative;
    width: 100%;
  }

  table {
    border: 0;
  }

  table, thead {
    clip: rect(0 0 0 0);
    position: absolute;
  }

  th, td {
    display: block;
    width: 100%;
    box-sizing: border-box;
    margin-bottom: 5px;
  }

  th {
    background-color: #f2f2f2;
  }

  .button-container {
    text-align: center;
  }

  .button {
    margin-top: 10px;
    background-color: #333;
    color: #fff;
    padding: 10px 20px;
    cursor: pointer;
    border: none;
    border-radius: 4px;
  }

  .button:hover {
    background-color: #555;
  }

}
</style>
