<template>
  <b-modal
    id="bv-modal-newpage"
    ref="modal"
    class="color-primary"
    title="Remove your profile photo?"
    centered
    hide-footer
  >
    <div class="d-block new-page-form">
      <div class="custom-form px-2 my-md-5 my-2"></div>
    </div>
    <div class="modal-footer">
      <a class="btn btn-primary" @click="remove">Remove</a>
      <a class="btn btn-outline" @click="$bvModal.hide('bv-modal-newpage')"
        >Cancel</a
      >
    </div>
  </b-modal>
</template>

<script>
import { mapActions } from "vuex";

export default {
  props: ["name"],
  data() {
    return {
      test: "Mak Ashtekar.,....",
    };
  },
  methods: {
    ...mapActions({ removeProfilePicture: "user/removePicture" }),
    show() {
      this.$refs["modal"].show();
    },
    hide() {
      this.$refs["modal"].hide();
    },
    remove() {
      this.removeProfilePicture()
        .then((response) => {
          this.$toast.success(response.data.message);
        })
        .catch((e) => {
          console.error("Remove Profile Picture error", e);
        });

      this.hide();
    },
  },
};
</script>

<style lang="scss">
</style>
