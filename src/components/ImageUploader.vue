<template>
  <div class="hello">
    <div class="form-group files text-center" ref="fileform">
      <input type="file" ref="file" multiple="multiple" />
      <span id="val"></span>
      <button class="btn" @click="submitFiles()" id="button">Upload Photo</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "ImageUploader",
  props: {
    msg: String,
  },

  methods: {
    submitFiles() {
      let formData = new FormData();

      for (var i = 0; i < this.$refs.file.files.length; i++) {
        let file = this.$refs.file.files[i];
        console.log(file);
        formData.append("files[" + i + "]", file);
      }

      axios
        .post("/fileupload", formData, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        })
        .then(function () {})
        .catch(function () {});
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
