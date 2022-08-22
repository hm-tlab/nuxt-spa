<template>
  <validation-observer
    ref="observer"
    v-slot="{ invalid }"
  >
    <form @submit.prevent="submit">
      <validation-provider
        v-slot="{ errors }"
        name="Name"
        rules="required|max:10"
      >
        <v-text-field
          v-model="name"
          :error-messages="errors"
          label="お名前"
          required
        ></v-text-field>
      </validation-provider>
      <validation-provider
        v-slot="{ errors }"
        name="phoneNumber"
        :rules="{
          digits: 11,
          regex: '^\\d{11}$'
        }"
      >
        <v-text-field
          v-model="phoneNumber"
          :counter="11"
          :error-messages="errors"
          label="電話番号"
        ></v-text-field>
      </validation-provider>
      <validation-provider
        v-slot="{ errors }"
        name="email"
        rules="required|email"
      >
        <v-text-field
          v-model="email"
          :error-messages="errors"
          label="E-mail"
          required
        ></v-text-field>
      </validation-provider>
      <validation-provider
        v-slot="{ errors }"
        name="inquiryMessage"
        rules="required"
      >
        <v-text-field
          v-model="inquiryMessage"
          :error-messages="errors"
          label="お問い合わせ本文"
          required
        ></v-text-field>
      </validation-provider>
      <v-btn
        class="mr-4"
        type="submit"
        :disabled="invalid"
      >
        submit
      </v-btn>
      <v-btn @click="clear">
        clear
      </v-btn>
    </form>
  </validation-observer>
</template>

<script>
  import { required, digits, email, max, regex } from 'vee-validate/dist/rules'
  import { extend, ValidationObserver, ValidationProvider, setInteractionMode } from 'vee-validate'
  setInteractionMode('eager')
  extend('digits', {
    ...digits,
    message: '半角英数 {length} 桁(ハイフン無し)で入力してください。',
  })
  extend('required', {
    ...required,
    message: 'このフィールドは必須です。',
  })
  extend('max', {
    ...max,
    message: '{length} 文字以内で入力してください',
  })
  extend('regex', {
    ...regex,
    message: '{_value_} は表現 {regex} にマッチしません。',
  })
  extend('email', {
    ...email,
    message: '正しいメールアドレスフォーマットで入力してください。',
  })
  export default {
    components: {
      ValidationProvider,
      ValidationObserver,
    },
    data: () => ({
      name: '',
      phoneNumber: '',
      email: '',
      inquiryMessage: ''
    }),
    methods: {
      submit () {
        this.$refs.observer.validate()
      },
      clear () {
        this.name = ''
        this.phoneNumber = ''
        this.email = ''
        this.inquiryMessage = ''
        this.$refs.observer.reset()
      },
    },
  }
</script>
