<template>
  <button @click="deleteCurrentRepo">Удалить репозиторий</button>
  <Popup ref="confirmationPopup">
    Вы действительно хотите удалить текущий репозиторий? Данное действие
    необратимо.
    <template #actions="{ confirm }">
      Напишите согласие
      <input :placeholder="$options.CONFIRMATION_TEXT" v-model="confirmation" />
      &nbsp;
      <button @click="confirm" :disabled="!isConfirmationCorrect">ОК</button>
    </template>
  </Popup>
</template>

<script>
import Popup from "./components/Popup.vue";
export default {
  components: { Popup },
  data() {
    return { confirmation: "" };
  },

  CONFIRMATION_TEXT: "ПОДТВЕРЖДАЮ",
  ALERT_TEXT: "Текущий репозиторий удален!",

  computed: {
    isConfirmationCorrect() {
      return this.confirmation === this.$options.CONFIRMATION_TEXT;
    },
  },

  methods: {
    async deleteCurrentRepo() {
      this.confirmation = "";
      // this.$refs нужны для модалок, и для фокуса на инпуте (на обертке инпута)
      const popupResult = await this.$refs.confirmationPopup.open();
      if (popupResult) {
        alert(this.$options.ALERT_TEXT);
      }
    },
  },
};
</script>