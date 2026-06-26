<template>
  <div class="gerenciamento-container">
    <AlertComponent :msg="msg" :type="alertType" />

    <div id="pedidos-tabela">
      <div>
        <div id="pedidos-tabela-cabecalho">
          <div id="ordem-id">#ID</div>
          <div>Nome</div>
          <div>Sushi</div>
          <div>Arroz</div>
          <div>Opcionais</div>
          <div>Status</div>
          <div id="div-acoes">Ações</div>
        </div>
      </div>
    </div>
    
    <div
      class="pedidos-tabela-linha"
      v-for="pedido in listaPedidosRealizados"
      :key="pedido.id"
    >
      <div id="ordem-numero">{{ pedido.id }}</div>
      <div>{{ pedido.nome }}</div>
      <div>{{ pedido.sushi && pedido.sushi.nome ? pedido.sushi.nome : 'Personalizado' }}</div>
      <div>{{ pedido.arroz && pedido.arroz.descricao ? pedido.arroz.descricao : '-' }}</div>
      <div>
        <ul>
          <li v-for="(complemento, index) in pedido.complementos" :key="index">
            {{ complemento.nome }}
          </li>
        </ul>
        <div class="divider" v-if="pedido.complementos?.length && pedido.bebidas?.length"></div>
        <ul>
          <li v-for="(bebida, index) in pedido.bebidas" :key="index">
            {{ bebida.nome }}
          </li>
        </ul>
      </div>
      <div>
        <select
          name="status"
          class="status"
          @change="atualizarStatusPedido($event, pedido.id)"
        >
          <option value="">Selecione</option>
          <option
            v-for="status in listaStatusPedido"
            :key="status.id"
            :value="status.id"
            :selected="status.id == pedido.statusId"
          >
            {{ status.descricao }}
          </option>
        </select>
      </div>
      <div id="div-acoes">
        <img
          @click="deletarPedido(pedido.id)"
          src="/img/icone_lixeira.png"
          alt="Excluir"
          width="30px"
          height="30px"
          class="btn-deletar"
        />
      </div>
    </div>
  </div>
</template>

<script>
import AlertComponent from "./AlertComponent.vue";

export default {
  name: "ListaPedidoComponent",
  components: {
    AlertComponent,
  },
  data() {
    return {
      listaPedidosRealizados: [],
      listaStatusPedido: [],
      msg: "",
      alertType: "info",
    };
  },
  methods: {
    async consultarPedidos() {
      const response = await fetch(`${this.$apiUrl}/pedidos`);
      const dados = await response.json();
      this.listaPedidosRealizados = dados;
    },
    async consultarStatusPedido() {
      const response = await fetch(`${this.$apiUrl}/status_pedido`);
      this.listaStatusPedido = await response.json();
    },
    async deletarPedido(id) {
      if (confirm("Deseja realmente cancelar este pedido?")) {
        await fetch(`${this.$apiUrl}/pedidos/${id}`, {
          method: "DELETE",
        });
        
        
        this.consultarPedidos();
        
    
        this.msg = `Pedido #${id} removido com sucesso!`;
        this.alertType = "success";
        
        setTimeout(() => (this.msg = ""), 3000);
      }
    },
    async atualizarStatusPedido(event, idPedido) {
      const idPedidoAtualizado = event.target.value;
      
      if (!idPedidoAtualizado) return;

      const atualizaoJson = JSON.stringify({ statusId: Number(idPedidoAtualizado) });

      await fetch(`${this.$apiUrl}/pedidos/${idPedido}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: atualizaoJson,
      });

    
      const statusEncontrado = this.listaStatusPedido.find(s => s.id == idPedidoAtualizado);
      const textoStatus = statusEncontrado ? statusEncontrado.descricao : "Atualizado";


      this.msg = `Pedido #${idPedido} alterado para: "${textoStatus}"`;
      this.alertType = "info"; 
      
      setTimeout(() => (this.msg = ""), 3000);
    },
  },
  mounted() {
    this.consultarPedidos();
    this.consultarStatusPedido();
  },
};
</script>

<style scoped>
.gerenciamento-container {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

#pedidos-tabela {
  width: 100%;
  margin: 0 auto;
}

#pedidos-tabela-cabecalho,
.pedidos-tabela-linha {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
}

#pedidos-tabela-cabecalho {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #222;
  background-color: #f5f5f5;
}

#pedidos-tabela-cabecalho div,
.pedidos-tabela-linha div {
  width: 18%;
}

.pedidos-tabela-linha {
  width: 100%;
  padding: 16px 12px;
  border-bottom: 1px solid #ddd;
  transition: 0.2s;
}

.pedidos-tabela-linha:hover {
  background-color: #fff8f7;
}

#pedidos-tabela-cabecalho #ordem-id,
.pedidos-tabela-linha #ordem-numero,
.pedidos-tabela-linha #div-acoes,
#pedidos-tabela-cabecalho #div-acoes {
  width: 5%;
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

li {
  font-size: 13px;
  color: #555;
}

.divider {
  height: 1px;
  background-color: #FA8072;
  margin: 6px 0;
  width: 80%;
}

.status {
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ccc;
  font-size: 13px;
  width: 90%;
}

.status:focus {
  border-color: #B30000;
  outline: none;
}

.btn-deletar {
  cursor: pointer;
  transition: transform 0.2s;
}

.btn-deletar:hover {
  transform: scale(1.1);
}
</style>