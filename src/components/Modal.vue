<template>
    <div class="modal fade" :class="{ show: showModal }" tabindex="-1" role="dialog">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Restart Game</h5>
                    <button type="button" class="close" @click="closeModal">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <p>Are you sure you want to restart the game?  All of your points and gameplay history will be lost forever.</p>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" @click="closeModal">Close</button>
                    <button type="button" class="btn btn-danger" @click="confirmRestart">Restart</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import event from "../event.js";
export default {
  name: "Modal",
  data() {
    return {
      showModal: false
    };
  },
  methods: {
    closeModal() {
      this.showModal = false;
    },
    confirmRestart() {
      event.$emit("restart");
      localStorage.clear();
      this.closeModal();
    }
  },
  mounted() {
    event.$on("show_modal", () => {
      this.showModal = true;
    });
  }
};
</script>

<style>
.show {
  display: block !important;
}
</style>
