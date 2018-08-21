<template>
  <div class="container" id="app">
    <h1 class="text-center">rock&bull;paper&bull;scissors</h1>
    <hr>
    <div class="row">
      <gameplay></gameplay>
      <score></score>
      <history></history>
    </div>
    <modal></modal>
    <hr>
    <copyright></copyright>
  </div>
</template>

<script>
import Gameplay from "./components/Gameplay.vue";
import Score from "./components/Score.vue";
import History from "./components/History.vue";
import Modal from "./components/Modal.vue";
import Copyright from "./components/Copyright.vue";
import "bootstrap/dist/css/bootstrap.min.css";
import "font-awesome/css/font-awesome.min.css";
import "./main.css";

export default {
  name: "app",
  components: {
    Modal,
    Gameplay,
    Score,
    History,
    Copyright
  }
};
</script>

<style>
body {
  background: #eaeaea;
}
</style>
