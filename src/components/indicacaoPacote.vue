<template>
    <nav>
      <div class="card-compras">
        <section class="card">
          <h2>Notas Fiscais</h2>
          <section class="notas-fiscais">
            <div>
              <p data-produto="Iniciante" @click="selecionarProduto('Iniciante')">Iniciante</p>
              <p data-produto="Intermediário" @click="selecionarProduto('Intermediário')">Intermediário</p>
              <p data-produto="Premium" @click="selecionarProduto('Premium')">Premium</p>
            </div>
          </section>
          <input type="range" min="0" max="2" step="1" v-model="valorRange.valor" id="rangeInput" />
        </section>
  
        <section class="card">
          <h2>Produtos</h2>
          <fieldset>
            <label for="financeiro">
              <input type="checkbox" id="financeiro" class="chk" v-model="produtosSelecionados.financeiro.selecionado">
              Financeiro
            </label>
            <label for="estoque">
              <input type="checkbox" id="estoque" class="chk" v-model="produtosSelecionados.estoque.selecionado">
              Estoque
            </label>
            <label for="dre">
              <input type="checkbox" id="dre" class="chk" v-model="produtosSelecionados.dre.selecionado">
              DRE
            </label>
            <label for="relatorio">
              <input type="checkbox" id="relatorio" class="chk" v-model="produtosSelecionados.relatorio.selecionado">
              Relatórios
            </label>
            <label for="suportePremium">
              <input type="checkbox" id="suportePremium" class="chk"
                v-model="produtosSelecionados.suportePremium.selecionado">
              Suporte Premium
            </label>
          </fieldset>
        </section>
      </div>
  
  
      <div class="card-resultado-pai">
        <section class="card-resultados" id="resultado">
          <ul>
            <li v-for="(value, key) in produtosSelecionados" :key="key" v-if="value">
              {{ key }}
            </li>
          </ul>
          <h2>Resultado</h2>
          <p v-if="comboSelecionado">
          <h3>o melhor plano pra voce é</h3>
          Combo Selecionado: {{ comboSelecionado }}
  
          </p>
          <div v-if="produtosSelecionadosCount > 1">
            <a class="el-content uk-button uk-button-primary botao-comprar" :href="gerarURLCompra()"
              v-if="!produtosSelecionados.suportePremium.selecionado && botaoComprarHabilitado">Comprar Agora</a>
            <a class="el-content uk-button uk-button-primary botao-modal" href="#js-115" uk-toggle=""
              v-if="produtosSelecionados.suportePremium.selecionado || !botaoComprarHabilitado">Solicite sua
              Demonstração</a>
          </div>
  
        </section>
      </div>
  
      <div id="js-115" class="uk-flex-top uk-modal" uk-modal="">
        <div class="uk-modal-dialog uk-width-auto uk-margin-auto-vertical">
          <button class="uk-modal-close-outside uk-icon uk-close" type="button" uk-close="">
            <svg width="14" height="14" viewBox="0 0 14 14" xmlns="http://www.w3.org/2000/svg"
              data-svg="close-icon"></svg>
          </button>
          <iframe :src="gerarURLIFrame()" id="modalIframe" width="800" height="600" frameborder="0" uk-responsive=""
            class="uk-responsive-width"></iframe>
        </div>
      </div>
    </nav>
  </template>
  
  <script>
  import { reactive, watch, ref } from 'vue';
  
  export default {
    setup() {
      const produtosSelecionados = reactive({
        financeiro: { id: '1', selecionado: false },
        estoque: { id: '2', selecionado: false },
        dre: { id: '3', selecionado: false },
        relatorio: { id: '4', selecionado: false },
        suportePremium: { id: '5', selecionado: false },
        iniciante: { id: '6', selecionado: true },
        intermediario: { id: '7', selecionado: false },
        premium: { id: '8', selecionado: false }
      });
  
      const valorRange = reactive({
        valor: '0'
      });
  
      let comboSelecionado = ref('');
      let botaoComprarHabilitado = ref(true);
  
      const combos = {
        ERP4ME: ['relatorio', 'iniciante'],
        ContábilFirst: ['estoque', 'dre', 'intermediario'],
        Moda: ['iniciante', 'dre', 'estoque'],
        Shop: ['premium', 'estoque'],
        Shop2: ['premium', 'estoque'],
        Iniciante_Estoque: ['iniciante', 'estoque'],
        Iniciante_DRE: ['iniciante', 'dre'],
        Iniciante_Relatorio: ['iniciante', 'relatorio'],
        Estoque_DRE: ['estoque', 'dre'],
        Estoque_Relatorio: ['estoque', 'relatorio'],
        DRE_Relatorio: ['dre', 'relatorio'],
        SuportePremium_Intermediario: ['suportePremium', 'intermediario'],
        SuportePremium_Premium: ['suportePremium', 'premium'],
        Iniciante_Estoque_DRE: ['iniciante', 'estoque', 'dre'],
        Iniciante_Estoque_Relatorio: ['iniciante', 'estoque', 'relatorio'],
        Iniciante_DRE_Relatorio: ['iniciante', 'dre', 'relatorio'],
        Estoque_DRE_Relatorio: ['estoque', 'dre', 'relatorio'],
        Iniciante_Estoque_DRE_Relatorio: ['iniciante', 'estoque', 'dre', 'relatorio'],
        Iniciante_SuportePremium: ['iniciante', 'suportePremium'],
        Estoque_SuportePremium: ['estoque', 'suportePremium'],
        DRE_SuportePremium: ['dre', 'suportePremium'],
        Relatorio_SuportePremium: ['relatorio', 'suportePremium'],
        Iniciante_Estoque_SuportePremium: ['iniciante', 'estoque', 'suportePremium'],
        Iniciante_DRE_SuportePremium: ['iniciante', 'dre', 'suportePremium'],
        Iniciante_Relatorio_SuportePremium: ['iniciante', 'relatorio', 'suportePremium'],
        Estoque_DRE_SuportePremium: ['estoque', 'dre', 'suportePremium'],
        Estoque_Relatorio_SuportePremium: ['estoque', 'relatorio', 'suportePremium'],
        DRE_Relatorio_SuportePremium: ['dre', 'relatorio', 'suportePremium'],
        Iniciante_Estoque_DRE_SuportePremium: ['iniciante', 'estoque', 'dre', 'suportePremium'],
        Iniciante_Estoque_Relatorio_SuportePremium: ['iniciante', 'estoque', 'relatorio', 'suportePremium'],
        Iniciante_DRE_Relatorio_SuportePremium: ['iniciante', 'dre', 'relatorio', 'suportePremium'],
        Estoque_DRE_Relatorio_SuportePremium: ['estoque', 'dre', 'relatorio', 'suportePremium'],
        Iniciante_Estoque_DRE_Relatorio_SuportePremium: ['iniciante', 'estoque', 'dre', 'relatorio', 'suportePremium']
      };
  
  
      function selecionarProduto(produto) {
        produtosSelecionados[produto.toLowerCase()].selecionado = !produtosSelecionados[produto.toLowerCase()].selecionado;
      }

  
      watch(() => valorRange.valor, (newVal, oldVal) => {
        if (newVal === '0') {
          produtosSelecionados.iniciante.selecionado = true;
          produtosSelecionados.intermediario.selecionado = false;
          produtosSelecionados.premium.selecionado = false;
        } else if (newVal === '1') {
          produtosSelecionados.iniciante.selecionado = false;
          produtosSelecionados.intermediario.selecionado = true;
          produtosSelecionados.premium.selecionado = false;
        } else if (newVal === '2') {
          produtosSelecionados.iniciante.selecionado = false;
          produtosSelecionados.intermediario.selecionado = false;
          produtosSelecionados.premium.selecionado = true;
        }
      });
  
  
      let produtosSelecionadosCount = ref(0)
      watch(() => produtosSelecionados, () => {
        const produtosSelecionados = Object.entries(produtosSelecionados).filter(([key, value]) => value.selecionado);
        produtosSelecionadosCount.value = produtosSelecionados.length;
  
  
        let comboEncontrado = '';
        for (const [nomeCombo, produtosCombo] of Object.entries(combos)) {
          const ComboProdutosSelecionados = produtosCombo.every(produto => produtosSelecionados.find(([key]) => key === produto));
          const semProdutoAdicional = produtosSelecionados.every(([key]) => produtosCombo.includes(key));
          if (ComboProdutosSelecionados && semProdutoAdicional) {
            comboEncontrado = nomeCombo;
            break;
          }
        }
        if (comboEncontrado !== '') {
          botaoComprarHabilitado.value = true
        } else {
          botaoComprarHabilitado.value = false
  
        }
        console.log(comboEncontrado);
        comboSelecionado.value = comboEncontrado;
      }, { deep: true });
  
  
  
      function gerarURLCompra() {
        const idsSelecionados = Object.entries(produtosSelecionados)
          .filter(([key, value]) => value.selecionado)
          .map(([key, value]) => value.id);
        const produtosSelecionadosIds = encodeURIComponent(idsSelecionados.join(','));
        return `https://www.google.com?produtos=${produtosSelecionadosIds}`;
      }
  
      function gerarURLIFrame() {
        const idsSelecionados = Object.entries(produtosSelecionados)
          .filter(([key, value]) => value.selecionado)
          .map(([key, value]) => value.id);
        const produtosSelecionadosIds = encodeURIComponent(idsSelecionados.join(','));
        return `https://app-mkt-leads.alterdatasoftware.com.br/formulario/contato/petshop?r=site-alterdata-empresarial&produtos=${produtosSelecionadosIds}`;
      }
  
      return { produtosSelecionados, valorRange, selecionarProduto, isRangeProdutoSelecionado, gerarURLCompra, gerarURLIFrame, comboSelecionado, botaoComprarHabilitado, produtosSelecionadosCount };
    }
  }
  
  </script>

  
