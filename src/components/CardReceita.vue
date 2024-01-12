<script lang="ts">
import Tag from './Tag.vue';
import type { PropType } from 'vue';
import IReceita from '@/interfaces/IReceita';

export default {
    props: {
        receita: { 
            type: Object as PropType<IReceita>,
            required: true
        },
        ingredientes: { 
            type: Array as PropType<string[]>, 
            required: true 
        }
    },
    components: {
        Tag,
    },
    methods: {
      getImageUrl(name: String) {
        const url = new URL(`../assets/imagens/cards_receitas/${name}`, import.meta.url);
        const semImagem =  new URL(`../assets/imagens/noimage.png`, import.meta.url);
        return url.pathname === "/undefined" ? semImagem : url
      }
    },
    emits: ['adicionarIngrediente', 'removerIngrediente']
  }
</script>

<template>
    <article class="card">
      <header class="categoria__cabecalho">
          <img :src="getImageUrl(receita.imagem)" alt="" class="categoria__imagem">
          <h2 class="paragrafo-lg categoria__nome">{{ receita.nome }}</h2>

          <ul class="categoria__ingredientes">
            <li v-for="ingrediente in receita.ingredientes" :key="ingrediente">
              <Tag :texto="ingrediente" :ativa="ingredientes.includes(ingrediente)"/>
            </li>
        </ul>
        </header>
    </article>
</template>

<style scoped>
.card {
  width: 20rem;
  border-radius: 1rem;
  background: var(--branco, #FFF);
  box-shadow: 4px 4px 10px 0px rgba(68, 68, 68, 0.05);
  height: 100%;

  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
  padding-bottom: 2rem;
}

.categoria__cabecalho {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
  width: 100%;
}

.categoria__imagem {
  width: 100%;
  border-radius: 1rem 1rem 0 0;
}

.categoria__nome {
  text-align: center;
  color: #000000;
  font-weight: 700;
  font-size: 1rem;
}

.categoria__ingredientes {
  display: flex;
  justify-content: center;
  gap: 0.5rem;
  flex-wrap: wrap;
}

</style>