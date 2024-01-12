<script lang="ts">
import { obterReceitas } from '@/http/index';
import IReceita from '@/interfaces/IReceita';
import CardReceita from './CardReceita.vue'
import BotaoPrincipal from './BotaoPrincipal.vue';
import type { PropType } from 'vue';
import { itensDeLista1EstaoEmLista2 } from '@/operacoes/listas';

export default {
    props: {
        ingredientes: { 
            type: Array as PropType<string[]>, 
            required: true 
        }
    },
    data() {
        return {
          receitas: [] as IReceita[]
        }
    },
    components: {
      CardReceita,
      BotaoPrincipal,
    },
    async created() {
        const receitasEncontradas = await obterReceitas();
        this.receitas = receitasEncontradas.filter((receita) => {
            const possoFazerReceita = itensDeLista1EstaoEmLista2(receita.ingredientes, this.ingredientes)
            return possoFazerReceita;
        })
    },
    emits: ['editarReceitas']
}
</script>
<template>
    <section class="receitas">
        <h1 class="cabecalho titulo">Receitas</h1>

        <div v-if="receitas.length" class="receitas">
            <p class="paragrafo contador">
                Resultados encontrados: {{ receitas.length }} 
            </p>
        
            <p class="paragrafo">
                Veja as opções de receitas com ingredientes que você tem por aí!
            </p>
    
            <ul class="cards">
                <li v-for="receita in receitas" :key="receita.nome">
                    <CardReceita :receita="receita" :ingredientes="ingredientes" />
                </li>
            </ul>
        </div>


        <div class="sem-resultado" v-else>
            <p class="paragrafo">Ops, não encontramos resultados para sua combinação. Vamos tentar de novo?</p>
            <img src="../assets/imagens/Empty-state.png" alt="Desenho de um ovo quebrado. A gema tem um rosto com uma expressão triste.">
        </div>

        <BotaoPrincipal :texto="'Editar lista'" @click="$emit('editarReceitas')" />
    </section>
</template>

<style scoped>
    .receitas{
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .cards{
        display: flex;
        justify-content: center;
        gap: 1rem;
        flex-wrap: wrap;
        padding: 1rem 0 3rem 0;
    }

    .sem-resultado{
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        padding-bottom: 2rem;
    }

    @media only screen and (max-width: 767px) {
        .receitas {
            margin-bottom: 2rem;
        }
    }


</style>