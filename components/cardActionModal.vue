<template>
  <Modal name="currentModal" v-model="show" @on-ok="ok" @on-cancel="cancel">
    <client-only placeholder="Cargando...">
      <article id="credit-box">
        <img v-bind:src="this.img" />
        <h3>{{ message.title }}</h3>
        <br />
        <br />
        <p>{{ message.message }}</p>
      </article>
    </client-only>
  </Modal>
</template>
<script>
import localStorage from "localStorage";
import { Modal } from "iview";
export default {
  name: "current-modal",
  components: {
    Modal
  },
  props: {
    show: Boolean,
    message: {},
    cards: Array,
    cardId: String
  },
  data() {
    return {
      img: ""
    };
  },
  beforeDestroy() {
    this.$Modal.remove();
  },
  computed: {
    readlyDate() {
      //   return moment(this.transaction.created).locale('es').format('dddd D MMMM YYYY')
    }
  },
  methods: {
    cancel() {
      this.$emit("close");
    },
    ok() {
      if (this.message.img == "default") {
        // this.img = "../assets/icons/Default card ico.svg";
        this.cards = JSON.parse(localStorage.getItem("cards"));
        this.cards.forEach(card => {
          if (card.cardNumber === this.cardId) {
            card.isDefault = true;
          } else {
            card.isDefault = false;
          }
        });
        console.log(this.cards);
        localStorage.setItem("cards", this.cards);
      } else {
        this.removeByAttr(
          JSON.parse(localStorage.getItem("cards")),
          "cardNumber",
          this.cardId
        );
        // this.img = "../assets/icons/Remove payment ico.svg";
      }
    },
    removeByAttr(arr, attr, value) {
      var i = arr.length;
      while (i--) {
        if (arr[i] && arr[i].hasOwnProperty(attr) && arr[i][attr] === value) {
          arr.splice(i, 1);
        }
      }
      return localStorage.setItem("cards", JSON.stringify(arr));
    },
    setDefault: function() {}
  }
};
</script>
<style scoped>
h3 {
  color: #192232;
}
.ivu-modal-mask {
  background-color: rgba(55, 55, 55, 0.85);
}
</style>
