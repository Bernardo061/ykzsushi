<template>
  <div>
    <form id="pedido-form" @submit="criarPedido($event)">
      <AlertComponent :msg="msg" :type="alertType" />

      <div>
        <p id="nome-sushi-content">
          {{ sushi && sushi.nome ? sushi.nome : "Monte seu Pedido" }}
        </p>
      </div>
      
      <div class="inputs">
        <label for="nome-cliente">Nome</label>
        <input
          type="text"
          v-model="nomeCliente"
          id="nome-cliente"
          name="nome-cliente"
          placeholder="Digite seu Nome"
        />
      </div>

      <div class="inputs">
        <label>Tipo de Arroz</label>
        <select name="tipo-arroz" id="tipo-arroz" v-model="tipoArrozSelecionado">
          <option value="" disabled selected>Selecione o tipo de arroz</option>
          <option
            v-for="arroz in listaTipoArroz"
            :key="arroz.id"
            :value="arroz"
          >
            {{ arroz.descricao }}
          </option>
        </select>
      </div>

      <div class="inputs">
        <label id="opcionais-titulo">Selecione os opcionais</label>
        <label id="opcionais-subtitulo">Complementos</label>

        <div
          class="checkbox-container"
          v-for="complemento in listaComplementos"
          :key="complemento.id"
        >
          <input
            type="checkbox"
            :name="complemento.nome"
            :value="complemento"
            v-model="listaComplementosSelecionados"
          />
          <span>{{ complemento.nome }}</span>
        </div>

        <label id="opcionais-subtitulo" style="margin-top: 15px;">Bebidas</label>

        <div
          class="checkbox-container"
          v-for="bebida in listaBebidas"
          :key="bebida.id"
        >
          <input
            type="checkbox"
            :name="bebida.nome"
            :value="bebida"
            v-model="listaBebidasSelecionadas"
          />
          <span>{{ bebida.nome }}</span>
        </div>

        <div class="inputs" style="margin-top: 25px;">
          <input type="submit" class="submit-btn" value="Confirmar Pedido" />
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import AlertComponent from "./AlertComponent.vue";

export default {
  name: "PedidoComponent",
  components: {
    AlertComponent,
  },
  props: {
    sushi: null,
  },
  data() {
    return {
      listaTipoArroz: [],
      listaBebidas: [],
      listaComplementos: [],
      nomeCliente: "",
      tipoArrozSelecionado: "",
      listaComplementosSelecionados: [],
      listaBebidasSelecionadas: [],
      msg: "",
      alertType: "info",
    };
  },
  methods: {
    async getTiposArroz() {
      const response = await fetch(`${this.$apiUrl}/tipos_arroz`);
      const dados = await response.json();
      this.listaTipoArroz = dados;
    },
    async getOpcionais() {
      const response = await fetch(`${this.$apiUrl}/opcionais`);
      const dados = await response.json();
      this.listaComplementos = dados.complemento;
      this.listaBebidas = dados.bebidas;
    },
    async criarPedido(e) {
      e.preventDefault();


      if (!this.nomeCliente.trim()) {
        this.msg = "Erro: O campo Nome do Cliente é obrigatório.";
        this.alertType = "error";
        window.scrollTo({ top: 0, behavior: "smooth" });
        return;
      }

      if (!this.tipoArrozSelecionado) {
        this.msg = "Erro: Selecione a opção base (Tipo de Arroz).";
        this.alertType = "error";
        window.scrollTo({ top: 0, behavior: "smooth" });
        return;
      }

  
      const dadosPedido = {
        nome: this.nomeCliente,
        arroz: this.tipoArrozSelecionado,
        bebidas: Array.from(this.listaBebidasSelecionadas),
        complementos: Array.from(this.listaComplementosSelecionados),
        sushi: this.sushi,
        statusId: 6, 
      };

      const dadosJson = JSON.stringify(dadosPedido);

      try {
        await fetch(`${this.$apiUrl}/pedidos`, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: dadosJson,
        });
        
        this.nomeCliente = "";
        this.tipoArrozSelecionado = "";
        this.listaComplementosSelecionados = [];
        this.listaBebidasSelecionadas = [];
        
        this.msg = "Pedido realizado com sucesso! Redirecionando para a cozinha...";
        this.alertType = "success";

        setTimeout(() => {
          this.msg = "";
          
          this.$router.push("/pedidos"); 
        }, 2000);

      } catch (error) {
        this.msg = "Erro ao processar o pedido com o servidor.";
        this.alertType = "error";
        
        setTimeout(() => {
          this.msg = "";
        }, 4000);
      }


      setTimeout(() => {
        this.msg = "";
      }, 4000);
    },
  },
  mounted() {
    this.getTiposArroz();
    this.getOpcionais();
  },
};
</script>

<style scoped>
#nome-sushi-content {
  font-size: 32px;
  font-weight: bold;
  text-align: center;
  margin-bottom: 30px;
  color: #222;
  padding: 16px;
  background-color: #f5f5f5;
  border-radius: 8px;
  border-bottom: 4px solid #B30000;
}

#pedido-form {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.inputs {
  display: flex;
  flex-direction: column;
  margin-bottom: 16px;
}

label {
  font-weight: bold;
  margin-bottom: 16px;
  color: #222;
  padding: 5px 12px;
  display: flex;
  border-left: 4px solid #B30000;
}

input[type="text"],
select {
  padding: 12px;
  width: 100%;
  border: solid #ccc 1px;
  border-radius: 8px;
  font-size: 14px;
}

select {
  height: 45px;
}

#opcionais-titulo {
  width: 100%;
  font-size: 18px;
}

#opcionais-subtitulo {
  display: flex;
  align-items: flex-start;
  width: 100%;
  margin-bottom: 12px;
  border-left: 4px solid #FA8072;
  font-size: 14px;
}

.checkbox-container {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.checkbox-container span {
  margin-left: 10px;
  font-weight: bold;
  color: #555;
}

.checkbox-container input {
  width: 18px;
  height: 18px;
  accent-color: #B30000;
}

.submit-btn {
  background-color: #222;
  color: #fff;
  font-weight: bold;
  border: none;
  font-size: 18px;
  border-radius: 8px;
  padding: 16px;
  margin: 0 auto;
  cursor: pointer;
  width: 100%;
  transition: 0.3s;
}

.submit-btn:hover {
  background-color: #B30000;
}
</style>