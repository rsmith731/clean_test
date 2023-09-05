<template>
  <b-modal
    id="bv-modal-newpage"
    ref="modal"
    class="color-primary"
    centered
    hide-footer
  >
    <div class="d-block new-page-form">
      <div class="custom-form px-2 my-md-5 my-2">
        <img
          v-if="profile.picture_path"
          :src="profile.picture_path"
          :alt="profile.name"
        />
        <p v-else>Profile picture not available</p>
      </div>
    </div>
  </b-modal>
</template>

<script>
import { mapActions, mapGetters } from "vuex";

export default {
  props: ["name"],
  data() {
    return {};
  },
  computed: {
    ...mapGetters({ profile: "user/profile" }),
  },
  methods: {
    show() {
      this.$refs["modal"].show();
    },
    hide() {
      this.$refs["modal"].hide();
    },
  },
};
</script>

<style lang="scss">
</style>
