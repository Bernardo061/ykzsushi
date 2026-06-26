<template>
  <div>
    <div id="main-banner">
      <h1>Seleção Premium do Mestre Sushi</h1>
    </div>

    <section id="destaques">
      <h2>Destaques do Cardápio</h2>
      
      <div class="cards-container">
        <div class="card" v-for="item in destaques" :key="item.id">
          <img :src="item.imagem" :alt="item.nome" />
          <div class="card-content">
            <h3>{{ item.nome }}</h3>
            <div class="card-footer">
              <span class="preco"> {{ item.preco }}</span>
              <button class="btn-adicionar">Adicionar</button>
            </div>
          </div>
        </div>
      </div>

      <div class="chef-info">
        <img src="/img/chef.png" alt="Chef Sushi" class="chef-img" />
        <div>
          <h3>A Arte de Fazer Sushi</h3>
          <p>Nossos mestres sushimen selecionam os peixes mais frescos para criar experiências autênticas.</p>
        </div>
      </div>

      <div class="cta-container">
        <router-link to="/menu" class="btn-cta">MONTE SEU PEDIDO PERSONALIZADO</router-link>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "HomeView", 
  data() {
    return {
      destaques: [
        {
          id: 1,
          nome: "Temaki Salmão Completo",
          preco: "R$ 39,90",
          imagem: "/img/temaki.png" 
        },
        {
          id: 2,
          nome: "Combinado Gourmet (20 un)",
          preco: "R$ 68,00",
          imagem: "/img/combinado.png"
        },
        {
          id: 3,
          nome: "Poke Salmão & Atum",
          preco: "R$ 52,50",
          imagem: "/img/poke.png"
        }
      ]
    };
  }
};
</script>

<style scoped>

#main-banner {
  
  background-image: url("../../public/img/banner.jpg"); 
  height: 300px;
  background-position: center;
  background-size: cover;
  display: flex;
  align-items: flex-end;
  justify-content: flex-end;
}

#main-banner h1 {
  font-size: 40px;
  color: rgb(252, 250, 248);
  background: #222; 
  padding: 25px;
  border-radius: 100px 0px 0px 100px;
  margin-bottom: 30px;
}


#destaques {
  padding: 40px 20px;
  background-color: #ffffff; 
  text-align: center;
}

#destaques h2 {
  font-size: 28px;
  color: #222;
  margin-bottom: 30px;
}


.cards-container {
  display: flex;
  justify-content: center;
  gap: 20px;
  flex-wrap: wrap;
  margin-bottom: 40px;
}

.card {
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  width: 250px;
  text-align: left;
}

.card img {
  width: 100%;
  height: 150px;
  object-fit: cover;
}

.card-content {
  padding: 15px;
}

.card-content h3 {
  font-size: 16px;
  color: #222;
  margin-bottom: 15px;
  height: 40px; 
}

.card-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.preco {
  font-size: 14px;
  color: #555;
}

.btn-adicionar {
  background-color: #B30000; 
  color: #fff;
  border: none;
  padding: 8px 15px;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
  transition: 0.3s;
}

.btn-adicionar:hover {
  background-color: #800000;
}

.chef-info {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
  max-width: 600px;
  margin: 0 auto 40px auto;
  text-align: left;
}

.chef-img {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  object-fit: cover;
}

.chef-info h3 {
  color: #222;
  margin-bottom: 5px;
}

.chef-info p {
  color: #666;
  font-size: 14px;
}

.cta-container {
  margin-top: 20px;
}

.btn-cta {
  display: inline-block;
  background-color: #B30000; 
  color: #fff;
  text-decoration: none;
  font-size: 18px;
  font-weight: bold;
  padding: 15px 40px;
  border-radius: 30px;
  transition: 0.3s;
  box-shadow: 0 4px 6px rgba(179, 0, 0, 0.3);
}

.btn-cta:hover {
  background-color: #800000;
  transform: translateY(-2px);
}
</style>