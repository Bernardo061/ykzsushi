<template>
  <div class="configuracao-container">
    <h1>Configuração do Pedido</h1>
    <pedido-component-vue :sushi="this.sushiSelecionado" />
  </div>
</template>

<script>
import PedidoComponentVue from "@/components/PedidoComponent.vue";

export default {
  name: "ConfiguracaoPedidoView",
  components: {
    PedidoComponentVue,
  },
  data() {
    return {
      sushiSelecionado: null,
    };
  },
  mounted() {
    const query = this.$route.query;
    // Alterado de burguer para sushi
    if (query.sushi) {
      const decodeSushi = JSON.parse(decodeURIComponent(query.sushi));
      this.sushiSelecionado = decodeSushi;
    }
  },
};
</script>

<style scoped>
.configuracao-container {
  padding: 40px 20px;
}
h1 {
  text-align: center;
  color: #222;
  margin-bottom: 40px;
}
</style>