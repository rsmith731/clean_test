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
        <form @submit.prevent="submitForm" id="profileImageUploadForm">
          <input
            type="file"
            @change="previewImage"
            ref="fileInput"
            class="d-none"
          />

          <div class="row">
            <div
              class="col image-selector"
              v-if="profileImageSrc == null"
              @click="selectImage"
            >
              Select picture
            </div>
            <div class="col image-preview" v-else>
              <img v-if="profileImageSrc" :src="profileImageSrc" />
            </div>
          </div>
        </form>
      </div>
    </div>
    <div class="modal-footer">
      <button
        class="btn btn-primary"
        v-if="profileImageSrc"
        type="submit"
        form="profileImageUploadForm"
      >
        Upload
      </button>
      <button class="btn btn-primary" v-else @click="remove">Remove</button>
      <a class="btn btn-outline" @click="cancel()">Cancel</a>
    </div>
  </b-modal>
</template>

<script>
import { Cropper } from "vue-advanced-cropper";
// Add the following line to import the cropper styles
import "vue-advanced-cropper/dist/style.css";

import { mapActions } from "vuex";

export default {
  props: ["name"],
  components: {
    copper: Cropper,
  },
  data() {
    return {
      profileImageSrc: null,
      formErrors: {},
    };
  },
  methods: {
    ...mapActions({ uploadPicture: "user/uploadPicture" }),
    show() {
      this.$refs["modal"].show();
    },
    hide() {
      this.$refs["modal"].hide();
    },
    remove() {
      this.hide();
    },
    selectImage() {
      this.$refs.fileInput.click();
    },
    previewImage(e) {
      const file = e.target.files[0];
      const reader = new FileReader();
      reader.onload = (e) => {
        this.profileImageSrc = e.target.result;
      };
      reader.readAsDataURL(file);
    },
    submitForm() {
      let formData = new FormData();
      formData.append("file", this.$refs.fileInput.files[0]);

      this.uploadPicture(formData)
        .then((response) => {
          this.$toast.success(response.data.message);
          this.cancel();
        })
        .catch((e) => {
          const { data } = e.response;
          this.$toast.error(data.message);
        });
    },
    cancel() {
      this.profileImageSrc = null;
      this.$bvModal.hide("bv-modal-newpage");
    },
  },
};
</script>

<style lang="scss" scoped>
@import "~assets/scss/variables.scss";

.image-selector {
  height: 20rem;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  border: dashed 1px $primary;
}

.image-preview {
  height: 20rem;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;

  img {
    height: 20rem;
  }
}
</style>
