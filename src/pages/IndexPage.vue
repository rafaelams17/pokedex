<template>
  <q-page class="flex flex-center">
    <div class="column items-center">
      <h2>{{ name }}</h2>
      <q-img :src="url" />
    </div>

    <div class="row justify-around full-width">
      <q-input label="Digite o nome de um pokemon" filled v-model="search" />
      <q-btn color="purple" label="Pesquisar" @click="getPokemon"/>
    </div>
  </q-page>
</template>

<script>
import api from "../services/api"
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'IndexPage', 

  data(){
    return{
      name: "", 
      url: "",
      search: "ditto"
    };
  },

  async beforeMount(){
    await this.getPokemon();
  }, 

  methods: {
    getPokemon() {
      api.get(`/pokemon/${this.search}/`)
      .then((response) => {
        // handle success
        this.name = response.data.name;
        this.url = response.data.sprites.other.dream_world.front_default;
        this.triggerPositive();
      })
      .catch((error) => {
        // handle error
        this.triggerNegative();
      })
    }, 
    triggerPositive () {
      this.$q.notify({
        type: 'positive',
        position: 'top',
        message: 'Pokemon encontrado ;)!'
      })
    },

    triggerNegative () {
      this.$q.notify({
        type: 'negative',
        position: 'top',
        message: 'Ocorreu um erro! Tente novamente ...'
      })
    },
  }
})
</script>
