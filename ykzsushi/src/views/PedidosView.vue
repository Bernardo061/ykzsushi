<template>
  <div class="pedidos-container">
    <h1>Gerenciar Pedidos</h1>
    <lista-pedido-component-vue />
  </div>
</template>

<script>
import ListaPedidoComponentVue from "@/components/ListaPedidoComponent.vue";

export default {
  name: "PedidosView",
  components: {
    ListaPedidoComponentVue,
  },
};
</script>

<style scoped>
.pedidos-container {
  padding: 40px 20px;
}
h1 {
  text-align: center;
  color: #222;
  margin-bottom: 40px;
}
</style>