<template>
  <div class="container">
    <div class="wrapper">
      <h2 class="login-title">Регистрация</h2>

      <div class="login-form">
        <div>
          <label for="email">Логин</label>
          <KitInput
            placeholder="Введите логин"
            :value="login"
            :onChange="(value) => (login = value)"
          />
        </div>

        <div>
          <label for="email">Имя</label>
          <KitInput
            placeholder="Введите ваше ися"
            :value="displayName"
            :onChange="(value) => (displayName = value)"
          />
        </div>

        <div>
          <label for="password">Пароль</label>
          <KitInput
            placeholder="Введите пароль"
            :value="password"
            :onChange="(value) => (password = value)"
            type="password"
          />
        </div>

        <div>
          <label for="password">Повторите пароль</label>
          <KitInput
            placeholder="Введите пароль"
            :value="repeatPassword"
            :onChange="(value) => (repeatPassword = value)"
            type="password"
            :error="repeatPasswordError || undefined"
          />
        </div>

        <div class="submit">
          <KitButton title="Зарегистрироваться" :onClick="onRegistration" />
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import KitButton from '@/shared/uiKit/Button.vue'
import KitInput from '@/shared/uiKit/Input.vue'
import { apiClient } from '@/app/api'
import type { AccessTokenPayload } from '@/shared/common/types'

export default {
  name: 'AuthPage',
  components: { KitButton, KitInput },
  data() {
    return {
      login: '',
      displayName: '',
      password: '',
      repeatPassword: '',
      repeatPasswordError: ''
    }
  },
  methods: {
    onRegistration() {
      if (this.password !== this.repeatPassword) {
        this.repeatPasswordError = 'Ошибка: пароли не совпадают'
      } else {
        this.repeatPasswordError = ''
        apiClient
          .post<{ accessToken: string; payload: AccessTokenPayload }>('/register', {
            username: this.login,
            password: this.password,
            displayname: this.displayName
          })
          .then(() => {
            this.$router.push({ name: 'auth' })
          })
          .catch((err) => {
            console.log({ err })
          })
      }

      // здесь будет регистрация и проверка пароля
    }
  }
}
</script>

<style lang="scss">
.container {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  font-family: Montserrat;
}

.wrapper {
  width: 400px;
  margin: auto;
  padding: 36px 48px 48px 48px;
  border: 1px solid #e9e9e9;

  border-radius: 11px;
}

.login-title {
  padding: 15px;
  font-size: 22px;
  text-align: center;
  font-family: MontserratBold;
}

.login-form {
  display: grid;
  grid-template-columns: 1fr;
  row-gap: 16px;
}

.login-form label {
  display: block;
  margin-bottom: 8px;
  font-size: 14px;
}

.login-form input {
  width: 100%;
  padding: 1.2rem;
  border-radius: 9px;
  border: none;
}

.login-form input:focus {
  outline: none;
}

.submit {
  width: 100%;
  margin-top: 16px;
  & > * {
    width: 100%;
  }
}
</style>
