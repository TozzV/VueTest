<template>
  <div class="container">

    <h1 class="title">{{ titulo }}</h1>

    <input type="search" class="filtro" v-on:input="filtro = $event.target.value" placeholder="filtre pelo título da foto">
    

    <ul class="lista-fotos">

      <li class="lista-fotos-item" v-for="foto in fotosComFiltro">

        <meu-painel :titulo="foto.titulo">
          <img class="imagem-responsiva" :src="foto.url" :alt="foto.titulo">
        </meu-painel>

      </li>
    </ul>

  </div>
</template>

<script>
import Painel from './components/shared/painel/painel.vue';

export default {

  components: {
    'meu-painel': Painel
  },
 
  data() {
    return {
      titulo: 'Teste VueJS',
      fotos: [   ],
      filtro:''

      
    }
  }, 

   computed: {
    fotosComFiltro() {
     if (this.filtro) {
          // criando uma expressão com o valor do filtro, insensitivo
        let exp = new RegExp(this.filtro.trim(), 'i');
        // retorna apenas as fotos que condizem com a expressão
        return this.fotos.filter(foto => exp.test(foto.titulo));
      } else {
        return this.fotos;
      }

    }
  },
   created(){
      this.$http.get('http://localhost:3000/v1/fotos')
      .then(res => res.json())
      .then(fotos => this.fotos = fotos, err => console.log(err));
  }
}
</script>

<style>
  .title {
    text-align: center;
  }

  .container {
    font-family: Helvetica, sans-serif;
    margin: 0 auto;
    width: 96%;

    background: #c8c8c8;
  }

  .lista-fotos {
    list-style: none;
  }

  .lista-fotos .lista-fotos-item {
    display: inline-block;
    padding: 1%;
  }

  .imagem-responsiva {
    width: 100%;
  }

    .filtro {
    display: block;
    width: 100%;
  }

</style>
