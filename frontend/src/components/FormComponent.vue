<template>
  <div id="form-container">
    <form id="submit-form" @submit.prevent="handleSubmit">
      <div id="select-container">
        <select id="select-field" v-model="selectedOption" required>
          <option value="" disabled selected>Выберите вариант</option>
          <option v-for="option in options" :key="option.id" :value="option.value">
            {{ option.title }}
          </option>
        </select>
      </div>
      <div id="email-field">
        <input v-model="email" required placeholder="Электронная почта"/>
        <div v-if="emailError" class="error">{{ emailError }}</div>
      </div>
      <button type="submit">Отправить</button>
    </form>

    <div v-if="isSent" id="success">Форма успешно отправлена!</div>
  </div>
</template>
  

<script lang="ts">
  import axios from 'axios';
  import { defineComponent, ref, onMounted } from 'vue';
  
  interface Option {
    id   : number;
    title: string;
    value: string;
  }
  
  export default defineComponent({
    name: 'FormComponent',
    setup() {
      const API_URL = 'http://localhost:1337/api';

      const options = ref<Option[]>([]);

      const selectedOption = ref('');
      const email = ref('');
      const emailError = ref('');
      const isSent = ref(false);

      onMounted( async () => {
        try {
          const response = await axios.get(`${API_URL}/options`)
          options.value = response.data.data.map((item: any) => ({
            id   : item.id,
            title: item.title,
            value: item.value
          }));
        } catch (error) {
          alert("Ошибка, попробуйте позже.");
          console.error('Ошибка при загрузке опций:', error);
        } 
      });
      
      function validateEmail() {
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        if (!emailRegex.test(email.value)) {
          emailError.value = 'Введите корректный email';
        } else {
          emailError.value = '';
        }
      }
  
      async function handleSubmit() {
        validateEmail();
        if (emailError.value) return;
          
        try {
          const content = JSON.stringify({
            option: selectedOption.value,
            email: email.value,
          })
          
          await axios.post(`${API_URL}/forms`, {
            data: {
              content: content
            }
          })
          
          isSent.value = true;
          selectedOption.value = '';
          email.value = '';
          
        } catch (error) {
          alert('Ошибка при отправке формы, попробуйте позже.');
          console.error('Ошибка при отправке формы:', error);
        }
      };
  
      return {
        selectedOption,
        email,
        handleSubmit,
        options,
        emailError,
        isSent
      };
    }
  });
</script>
  
<style>
  #form-container {
    width: 100%;
    padding: 10px 235px 10px 10px;
  }

  #select-container select, 
  #email-field input {
    width: 100%;
    border-radius: 8px;
    border: 1px solid gray;
    background-color: var(--color-background-white);
    color: var(--color-text);
    margin-bottom: 10px;
    padding: 8px;
  }

  button {
    background-color: var(--color-button);
    margin-top: 10px;
    width: 100%;
    height: 44px;
    border: none;
    border-radius: 8px;
    color: var(--color-background-white);
    font-weight: bold;
    font-size: 16px;
  }

  button:hover {
    cursor: pointer;
    background-color: var(--color-button-hover);
  }

  #success {
    color: green;
    margin-top: 10px;
  }

  @media (width <= 900px) {
    #form-container {
      padding: 0;
    }
  }
  @media (900px <= width <= 1300px) {
    #form-container {
      padding: 10px 80px 10px 10px;
    }
  }
</style>