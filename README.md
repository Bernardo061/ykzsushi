# 🍣 YKZ Sushi — Sistema de Pedidos e Gerenciamento

Sistema de **pedidos e gerenciamento para restaurante de sushi**, desenvolvido em **Vue 3** como projeto final de Desenvolvimento Web. O projeto evolui a base estrutural do sistema **T-Burguer** (trabalhada em sala) para um novo segmento de negócio: **culinária japonesa**.

> Em vez de montar um hambúrguer, o cliente escolhe o seu combinado e finaliza o pedido selecionando a **opção base (tipo de arroz)**, **complementos** e **bebidas**.

---

## 🔗 Links

| Item                         | URL                                                        |
| ---------------------------- | ---------------------------------------------------------- |
| 🌐 **Produção (Vercel)** | _https://ykzsushi.vercel.app/_ |
| 🛰️ **API (JSON Server / Render)** | _https://api-ykzsushi.onrender.com_ |
| 💻 **Repositório** | _https://github.com/Bernardo061/ykzsushi_|

---

## 1. Visão Geral — do T-Burguer ao YKZ Sushi

Partindo da estrutura original (Vue 3 + Vue Router + JSON Server), o sistema foi **totalmente reidentificado** e teve seus dados, campos e regras adaptados ao novo segmento.

### Principais alterações estruturais

| Antes (T-Burguer)                | Agora (YKZ Sushi)                                   |
| -------------------------------- | --------------------------------------------------- |
| Hamburgueria                     | Restaurante de Sushi                                |
| Campo **"Ponto da carne"**       | Campo **"Tipo de Arroz"**  (obrigatório)               |
| "Selecione um hambúrguer"        | "Menu YKZ Sushi" / "Monte seu dido"                 |
| `/menu` → `burgues`              | `/menu` → `sushis`                                  |
| `/tipos_pontos`                  | `/tipos_arroz`                                      |
| Logo/banner do hambúrguer        | Identidade visual minimalista focada na cultura oriental |

### Novo campo obrigatório (substituindo "Ponto da carne")

```vue
<div class="inputs">
  <label>Tipo de Arroz *</label>
  <select name="tipo-arroz" id="tipo-arroz" v-model="tipoArrozSelecionado">
    <option value="" disabled selected>Selecione o tipo de arroz</option>
    <option v-for="arroz in listaTipoArroz" :key="arroz.id" :value="arroz">
      {{ arroz.descricao }}
    </option>
  </select>
</div>
Nova Identidade Visual
Paleta de Cores (60/30/10): Tons neutros focados em Preto (#222) e fundo Claro (#f5f5f5), com detalhes fortes em Vermelho (#B30000) e Salmão (#FA8072).

Design Minimalista: Os cards do menu focam em tipografia legível e descrições claras, abandonando excesso de imagens para um carregamento mais rápido e elegante.

2. Solução Técnica dos Alertas Semânticos
Toda a comunicação visual passa por um componente reutilizável, o AlertComponent.vue (ou similar), controlado de forma dinâmica pelo componente pai.

O Componente de Alerta
Ele recebe duas props — type (tipo) e msg (mensagem) — e, a partir do tipo, escolhe a cor semântica e o ícone. Quando a mensagem está vazia, o v-if simplesmente não renderiza nada:

Snippet de código
<template>
  <div class="alert-container" :class="type" v-if="msg">
    <span class="alert-icon">{{ icon }}</span>
    <p>{{ msg }}</p>
  </div>
</template>

<script>
export default {
  props: {
    msg: String,
    type: { type: String, default: "info" },
  },
  computed: {
    icon() {
      const mapa = {
        error: "❌", warning: "⚠️", info: "ℹ️", success: "✅"
      };
      return mapa[this.type] || mapa.info;
    },
  },
};
</script>
A paleta semântica (CSS)
Cada tipo aplica uma classe com a cor exigida:

CSS
.error   { background: #f8d7da; border-color: #f5c6cb; color: #721c24; } /* Vermelho */
.warning { background: #fff3cd; border-color: #ffeeba; color: #856404; } /* Laranja */
.info    { background: #d1ecf1; border-color: #bee5eb; color: #0c5460; } /* Azul */
.success { background: #d4edda; border-color: #c3e6cb; color: #155724; } /* Verde */
Como o alerta é disparado (Validação)
Antes de enviar o pedido, a função criarPedido() checa os campos obrigatórios e, se algo falta, dispara um alerta vermelho e bloqueia o envio:

JavaScript
// src/components/PedidoComponent.vue
async criarPedido(e) {
  e.preventDefault();

  if (!this.nomeCliente.trim()) {
    this.msg = "Erro: O campo Nome do Cliente é obrigatório.";
    this.alertType = "error"; // 🔴 bloqueia e avisa
    window.scrollTo({ top: 0, behavior: "smooth" });
    return; 
  }

  if (!this.tipoArrozSelecionado) {
    this.msg = "Erro: Selecione a opção base (Tipo de Arroz).";
    this.alertType = "error"; // 🔴 bloqueia e avisa
    window.scrollTo({ top: 0, behavior: "smooth" });
    return;
  }
  
  // ...envia o POST e mostra alerta verde de sucesso
}
3. Experiência do Usuário (UX)
Redirecionamento inteligente após o pedido
Ao confirmar com sucesso, o sistema exibe o alerta verde e, em seguida, navega automaticamente para a tela de gerenciamento de pedidos (cozinha):

JavaScript
this.msg = "Pedido realizado com sucesso! Redirecionando para a cozinha...";
this.alertType = "success";

setTimeout(() => {
  this.msg = "";
  this.$router.push("/pedidos"); // navegação automática
}, 2000);
Atualização em tempo real e feedback de remoção
A tela da cozinha busca os dados no mounted(), então o novo registro já aparece ao chegar. Na exclusão, o código invoca this.consultarPedidos() imediatamente, re-renderizando a lista na hora, e um alerta exibe a confirmação da ação:

JavaScript
// src/components/ListaPedidoComponent.vue
async deletarPedido(id) {
  if (confirm("Deseja realmente cancelar este pedido?")) {
    await fetch(`${this.$apiUrl}/pedidos/${id}`, { method: "DELETE" });
    
    // Re-renderização imediata
    this.consultarPedidos();
    
    // Alerta de sucesso
    this.msg = `Pedido #${id} removido com sucesso!`;
    this.alertType = "success";
  }
}
4. Estrutura do projeto
t-sushi/
├── db/db.json                       # base de dados do JSON Server
└── src/
    ├── App.vue
    ├── main.js                      # registra $apiUrl global
    ├── router/index.js              # rotas principais do sistema
    ├── components/
    │   ├── AlertComponent.vue        # ⭐ alerta semântico reutilizável
    │   ├── PedidoComponent.vue       # formulário + validação + redirect
    │   └── ListaPedidoComponent.vue  # tabela da cozinha + atualização em tempo real
    └── views/
        ├── MenuView.vue              # menu adaptado de hambúrguer para sushis
        ├── ConfiguracaoPedidoView.vue
        └── PedidosView.vue
5. Como rodar localmente
Pré-requisito: Node.js instalado.

Bash
# 1. instalar dependências
npm install

# 2. subir a API mockada (porta 3000) — deixe rodando em um terminal
npm run serve:api  # (ou o comando respectivo configurado no package.json)

# 3. em outro terminal, subir o front
npm run serve
Projeto acadêmico — Desenvolvimento Web · Vue 3.