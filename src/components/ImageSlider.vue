<template>
  <div class="form-group m-form__group row">
    <div class="col-lg-4">
      <div class="component-wrapper">
        <div class="image-previewer">
          <div v-for="i in [currentIndex]" :key="i">
            <img :src="currentImg" />
          </div>
        </div>

        <div class="action-menu">
          <form class="md-form" action="#">
            <div class="file-field">
              <div class="btn btn-primary btn-sm float-left">
                <span>Choose files</span>
                <input
                  id="file-upload"
                  ref="file"
                  class="custom-file-input"
                  multiple
                  name="document_file"
                  type="file"
                  @change="setFile($event)"
                />
              </div>
            </div>

            <div class="btn-group" role="group" aria-label="Basic example">
              <button type="button" class="btn btn-secondary" @click="previousImage()">Previous</button>
              <button type="button" class="btn btn-secondary" @click="nextImage()">Next</button>
              <button
                type="button"
                class="btn btn-danger"
                :id="currentImgId"
                @click="deleteFile(currentImgId)"
              >Delete</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ImageUploader",

  data() {
    return {
      imageUrls: [],
      images: [],
      img_id: 0,
      currentIndex: 0,
    };
  },

  computed: {
    currentImg() {
      if (this.imageUrls.length > 0) {
        return this.imageUrls[
          Math.abs(this.currentIndex) % this.imageUrls.length
        ].file;
      }
      return 0;
    },
    currentImgId() {
      if (this.imageUrls.length > 0) {
        return this.imageUrls[
          Math.abs(this.currentIndex) % this.imageUrls.length
        ].id;
      }
      return 0;
    },
  },

  methods: {
    setFile(e) {
      let self = this;
      let selectedFiles = e.target.files;

      for (let i = 0; i < selectedFiles.length; i++) {
        console.log(selectedFiles[i]);
        this.images.push(selectedFiles[i]);
      }

      for (let i = 0; i < this.images.length; i++) {
        let reader = new FileReader();
        reader.onload = () => {
          self.imageUrls.push({
            id: self.img_id,
            file: reader.result,
          });
          self.img_id++;
        };

        reader.readAsDataURL(this.images[i]);
      }
      console.log("imageUrls: ", self.imageUrls);
    },
    deleteFile(img_id) {
      let self = this;

      self.imageUrls = self.imageUrls.filter(function (obj) {
        return obj.id !== img_id;
      });
      console.log(img_id, self.imageUrls);

      // self.imageUrls.forEach(function (object){
      //     if(object.id == img_id){
      //         self.imageUrls.splice(object, 1);
      //     }
      // })
      // console.log(img_id, self.imageUrls);
    },

    nextImage() {
      this.currentIndex += 1;
    },

    previousImage() {
      this.currentIndex -= 1;
    },
  },
};
</script>

<style scoped>
img {
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 5px;
  width: 150px;
  height: 150px;
}

img:hover {
  box-shadow: 0 0 2px 1px rgba(0, 140, 186, 0.5);
}
</style>
