<template>
  <div id="app-container">
    <div id="description">
      <h3 style="font-weight: bold;">Заполните форму</h3>
      <p>На основе ваших ответов теста мы сформируем рекомендации
        для применения инструментов у вас в компании </p>
    </div>
    <FormComponent />
  </div>
</template>


<script lang="ts">
import FormComponent from './components/FormComponent.vue'
export default {
  name: 'App',
  components: {
    FormComponent
  }
}
</script>

<style>
  #app-container {
    padding: 2rem;
    font-weight: normal;
    background-color: var(--color-background-white);
    width: 95%;
    display: grid;
    grid-template-columns: 1fr 1fr;
    border-radius: 16px;
    padding: 32px 32px 64px 32px;
  }
  
  #description {
    padding: 10px 10px 10px 235px;
    text-align: left;
    font-size: 20px;
  }

  @media (width <= 900px) {
    #app-container {
      grid-template-columns: 1fr;
    }
    
    #description {
      padding: 0 0 30px 0;
    }
  }
  @media (900px <= width <= 1300px) {
    #description {
      padding: 10px 10px 10px 80px;
    }
  }
</style>