<template>
  <transition name="modal">
    <div @click="$emit('close')" class="modal-mask">
      <div class="modal-wrapper">
        <div @click.stop class="modal-container">
          <ul>
            <li v-for="(history, i) in this.histories[this.player]" :key="i">
              <span :class="!history.active ? 'gray' : ''">{{
                history.value
              }}</span>
              <input
                v-if="history.type !== 'SET'"
                id="check"
                type="checkbox"
                @click="changeActive($event, i)"
                :checked="history.active ? 'checked' : ''"
              />
            </li>
          </ul>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import { createNamespacedHelpers } from "vuex";

const { mapGetters, mapState, mapActions } = createNamespacedHelpers("life");

export default {
  name: "EditHistory",
  props: ["player"],
  methods: {
    changeActive(event, index) {
      this.modifyHistoryActive([
        this.player,
        index,
        !this.histories[this.player][index].active,
      ]);
    },
    ...mapActions(["modifyHistoryActive"]),
  },
  computed: {
    ...mapGetters(["players"]),
    ...mapState(["histories"]),
  },
};
</script>

<style scoped>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.gray {
  color: darkgrey;
}

.modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
}

/*
     * The following styles are auto-applied to elements with
     * transition="modal" when their visibility is toggled
     * by Vue.js.
     *
     * You can easily play with the modal transition by editing
     * these styles.
     */

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>
