<template>
  <div id="app">
    <nav>
      <div class="item-nav">
        <img
          alt="Mis aplicaciones"
          src="../assets/icons/My applications ico.svg"
        />
        <span class="desktop-view item-title">Mis aplicaciones</span>
      </div>
      <div class="item-nav">
        <img alt="Mis viajeros" src="../assets/icons/My travelers ico.svg" />
        <span class="item-nav-title">Mis viajeros</span>
      </div>
      <div class="item-nav active">
        <img alt="Mis tarjetas" src="../assets/icons/My cards ico.svg" />
        <span class="item-nav-title">Mis tarjetas</span>
      </div>
      <div class="item-nav">
        <img alt="Mi cuenta" src="../assets/icons/My account ico.svg" />
        <span class="item-nav-title">Mi cuenta</span>
      </div>
    </nav>
    <div class="my-wallet">
      <CardList />
    </div>
  </div>
</template>

<script>
import CardList from "../components/CardList.vue";
import "../assets/css/style.css";
export default {
  name: "app",
  layout: "default",
  components: {
    CardList
  }
};
</script>
