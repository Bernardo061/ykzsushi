<template>
  <div class="menu-container">
    <h1>Menu YKZ Sushi</h1>
    <div id="scroll-horizontal">
      <div
        id="card-content"
        v-for="sushi in listaMenuSushis"
        :key="sushi.id"
      >
        <div id="card-linha">
          <div class="card-sushi">
            <div class="card-coluna">
              <p id="nome-content">{{ sushi.nome }}</p>
              <div class="divider"></div>
              <p id="preco-content">R$ {{ sushi.valor }},00</p>
              <p id="descricao-content">{{ sushi.descricao }}</p>
              <button @click="selecionarSushi(sushi)">Selecionar</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MenuView",
  data() {
    return {
      listaMenuSushis: [],
    };
  },
  methods: {
    async consultarMenu() {
      const response = await fetch(`${this.$apiUrl}/menu`);
      const dados = await response.json();
      
  
      this.listaMenuSushis = dados.sushis; 
      console.log(this.listaMenuSushis);
    },
    selecionarSushi(sushiSelecionado) {
      const param = JSON.stringify(sushiSelecionado);
      const sushiJson = encodeURIComponent(param);
      
  
      this.$router.push({ path: "/config", query: { sushi: sushiJson } });
    },
  },
  mounted() {
    this.consultarMenu();
  },
};
</script>

<style scoped>
.menu-container {
  padding: 20px;
}

h1 {
  text-align: center;
  color: #222;
  margin-bottom: 30px;
}

#scroll-horizontal {
  display: flex;
  overflow-x: auto;
  gap: 20px;
  padding: 20px 10px;
  width: 100%;
  max-width: 900px;
  margin: 0 auto;
  scrollbar-width: thin;
  scrollbar-color: #B30000 #f5f5f5;
}

#card-content {
  flex: 0 0 auto;
  width: 280px;
  min-height: 350px; 
  background-color: #fff;
  border: 1px solid #ddd;
  border-top: 6px solid #B30000; 
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: transform 0.3s;
}

#card-content:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 15px rgba(0,0,0,0.15);
}

.card-sushi {
  display: flex;
  flex-direction: column;
  height: 100%;
  padding: 20px;
}

.card-coluna {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}

#nome-content {
  font-size: 24px;
  font-weight: bold;
  text-align: center;
  color: #222; 
  margin-bottom: 10px;
}

.divider {
  width: 50px;
  height: 3px;
  background-color: #FA8072; 
  margin: 0 auto 15px auto;
}

#preco-content {
  font-size: 28px;
  text-align: center;
  font-weight: bold;
  color: #B30000; 
  margin-bottom: 16px;
}

#descricao-content {
  font-size: 15px;
  text-align: center;
  color: #555;
  flex-grow: 1;
  white-space: pre-line;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 5;
  -webkit-box-orient: vertical;
}

.card-coluna button {
  margin-top: 20px;
  padding: 12px;
  background-color: #222; 
  color: #fff;
  font-weight: bold;
  border-radius: 8px;
  border: 2px solid #222;
  font-size: 16px;
  width: 100%;
  cursor: pointer;
  transition: 0.3s;
}

.card-coluna button:hover {
  background-color: #B30000; 
  border-color: #B30000;
  color: #fff;
}
</style>