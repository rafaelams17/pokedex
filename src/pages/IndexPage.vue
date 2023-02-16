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

    <div class="row justify-between full-width">
      <q-icon name="fa-solid fa-arrow-right" color="primary" class="q-ml-sm cursor-pointer container-arrows" size="4rem" @click="getPokemon(id - 1)">
        <q-tooltip>Anterior</q-tooltip>
      </q-icon>

      <q-icon name="fa-solid fa-arrow-left" color="primary" class="q-mr-sm cursor-pointer container-arrows" size="4rem" @click="getPokemon(id + 1)">
        <q-tooltip>Próximo</q-tooltip>
      </q-icon>

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
      id: null,
      search: "ditto"
    };
  },

  async beforeMount(){
    await this.getPokemon();
  },

  methods: {
    getPokemon(id) {
      api.get(id > 0 ? `/pokemon/${id}/` : `/pokemon/${this.search}/`)
      .then((response) => {
        // handle success
        this.id = response.data.id;
        this.name = response.data.name;
        this.search = response.data.name;
        this.url = response.data.sprites.other.dream_world.front_default;
        //this.triggerPositive();
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
