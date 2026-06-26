<template>
  <div class="alert-container" :class="type" v-if="msg">
    <span class="alert-icon">{{ icon }}</span>
    <p>{{ msg }}</p>
  </div>
</template>

<script>
export default {
  name: "AlertComponent",
  props: {
    msg: String,
    type: {
      type: String,
      default: "info", 
    },
  },
  computed: {
    icon() {
      switch (this.type) {
        case "success":
          return "✅"; 
        case "error":
          return "❌"; 
        case "warning":
          return "⚠️"; 
        case "info":
        default:
          return "ℹ️"; 
      }
    },
  },
};
</script>

<style scoped>
.alert-container {
  display: flex;
  align-items: center;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
  font-weight: bold;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  transition: 0.3s;
}

.alert-icon {
  font-size: 20px;
  margin-right: 10px;
}

.alert-container p {
  margin: 0;
}




.success {
  background-color: #d4edda;
  color: #155724;
  border: 1px solid #c3e6cb;
}

.error {
  background-color: #f8d7da;
  color: #721c24;
  border: 1px solid #f5c6cb;
}

.warning {
  background-color: #fff3cd;
  color: #856404;
  border: 1px solid #ffeeba;
}

.info {
  background-color: #d1ecf1;
  color: #0c5460;
  border: 1px solid #bee5eb;
}
</style>