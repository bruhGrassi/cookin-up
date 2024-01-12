<script lang="ts">
import Tag from './Tag.vue';
import type { PropType } from 'vue';

export default {
    props: {
        ingredientes: {
            type: Array as PropType<string[]>,
            required: true
        }
    },
    components: {
        Tag,
    }
  }
</script>

<template>
    <section>
        <span class="subtitulo-lg sua-lista-texto">
            Sua lista: 
        </span>

        <ul v-if="ingredientes.length" class="ingredientes-sua-lista">
            <li v-for="ingrediente in ingredientes" :key="ingrediente" class="ingrediente">
                <Tag :texto="ingrediente" ativa  />
            </li>
        </ul>
        <p v-else class="paragrafo lista-vazia">
            <img src="/public/Imagens/Empty-state.png" alt="Ícone de pesquisa">
            Sua lista está vazia, selecione ingredientes para iniciar.
        </p>
    </section>
</template>

<style scoped>

.ingredientes-sua-lista {
  display: flex;
  justify-content: center;
  gap: 1rem 1.5rem;
  flex-wrap: wrap;
}

.lista-vazia {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  flex-direction: column;
  gap: 0.25rem;

  color: var(--coral, #F0633C);
  text-align: center;
}

.lista-vazia img{
    width: 5rem;
}


.sua-lista-texto {
  color: var(--coral, #F0633C);
  display: block;
  text-align: center;
  margin-bottom: 1.5rem;
}
</style>