<template>
  <div id="app">
    <div class="container-fluid d-flex flex-column align-items-center">
      <div class="container">
        <h4 class="text-center">Controle de despesas</h4>
        <div class="div">
          <h5>SALDO ATUAL</h5>
          <h2>R$ {{ valorTotal }}</h2>
        </div>
        <div class="d-lg-flex d-column main text-center">
          <div class="col p-4 receitas">
            <h5>RECEITAS</h5>
            <h4>R$ {{ saldoReceita }}</h4>
          </div>
          <div class="col despesas border-left p-4 border-top">
            <h5>DESPESAS</h5>
            <h4>R$ {{ saldoDespesas }}</h4>
          </div>
        </div>
        <h5 class="border-bottom border-dark pt-5 pb-2">
          <strong>Transações</strong>
        </h5>
        <div>
          <div
            class="plus p-1 mt-3 d-flex justify-content-between"
            v-for="(transaction, index) in transactions"
            :key="index"
          >
            <button @click="deleteEvent(index)" class="btn bg-danger ml-0 mr-2 btn-close" style="color: white">
              x
            </button>
            <div class="d-flex align-items-center">{{ transaction.name }}</div>
            <div class="d-flex justify-content-end align-items-center col-10">
              R$ {{ transaction.receita }}
            </div>
          </div>
          <div
            class="negative p-1 mt-3 d-flex justify-content-between"
            v-for="(transaction, index) in transactions2"
            :key="index"
          >
            <button @click="deleteEvent2(index)" class="btn bg-danger ml-0 mr-2 btn-close" style="color: white">
              x
            </button>
            <div class="d-flex align-items-center">{{ transaction.name2 }}</div>
            <div class="d-flex justify-content-end align-items-center col-10">
              R$ {{ transaction.despesa }}
            </div>
          </div>
        </div>
        <h5 class="border-bottom border-dark pt-5 pb-2">
          <strong>Adicionar transação</strong>
        </h5>
        <div class="div">
          <label for="nome"></label>
          <input
            v-model="produtoTransacao"
            @keyup.enter="getProduto"
            class="w-100 p-1"
            type="text"
            id="nome"
            placeholder="Nome da transação"
          />
          <p class="pb-0 pt-3">Valor: (negativo-despesas, positivo-receitas)</p>
          <input
            type="number"
            v-model="valorTransacao"
            @keyup.enter="getValor"
            class="w-100 p-1"
            id="nome"
            placeholder="Valor da transação"
          />
          <buttton
            @click="getValor()"
            class="btn pt-3 my-4 w-100 bg-success text-center"
            style="color: white"
          >
            <strong>Adicionar</strong></buttton
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
emits: ['remove'],
  data: function () {
    return {
      transactions: [],
      transactions2: [],
      produtoTransacao: "",
      valorTransacao: "",
    };
  },
  methods: {
    getValor() {
      if (this.produtoTransacao == "" || this.valorTransacao == "") {
        alert("por favor, preencha tanto o nome quanto o valor da transação");
      } else if (this.valorTransacao < 0 && this.produtoTransacao !== 0) {
        this.transactions2.push({
          despesa: this.valorTransacao,
          name2: this.produtoTransacao,
        });
        this.valorTransacao = "";
        this.produtoTransacao = "";
      } else if (this.valorTransacao >= 0) {
        this.transactions.push({
          receita: this.valorTransacao,
          name: this.produtoTransacao,
        });
        this.valorTransacao = "";
        this.produtoTransacao = "";
      }
      const amount = parseInt(this.valorTransacao);
      if (amount) {
        this.transactions = [...this.transactions, { amount }];
        this.valorTransacao = 0;
      }
    },
    deleteEvent: function(index) {
                this.transactions.splice(index, 1);
        },
        deleteEvent2: function(index) {
                this.transactions2.splice(index, 1);
        },
  },

  computed: {
    saldoDespesas() {
      return this.transactions2.reduce((a, b) => {
        return a - +b.despesa;
      }, 0);
    },
    saldoReceita() {
      return this.transactions.reduce((a, b) => {
        return a - -b.receita;
      }, 0);
    },
    valorTotal() {
      return this.saldoReceita - this.saldoDespesas;
    },
  },
};
</script>

<style scoped>

.btn-close{
   margin-left: -35px !important;
}


</style>
