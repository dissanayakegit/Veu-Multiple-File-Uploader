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

  mounted(){
    let savedImages = []; // array of file urls returend from backend
    //backend method
    /*filesysyem.php
     'record_keeper' => [
            'driver' => 'local',
            'root' => storage_path('app/public/uploads/'.RECORD_KEEPER_UPLOAD_DIRECTORY)
        ],

    public function getImages()
    {
        $filesToReturn = [];
        $files = File::files(storage_path() . "/app/public/uploads/record_keeper");
        foreach ($files as $file) {
            $fileName = basename($file);
            $file = Storage::url('uploads/' .RECORD_KEEPER_UPLOAD_DIRECTORY . "/" . $fileName);
            array_push($filesToReturn, url($file));
        }
        return $this->apiResponse(['files' => $filesToReturn], $this->response_status_code, 200);
    }
    */
    //this.convertUploadedFilesToBase64();
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
      
      for (let i = 0; i < e.target.files.length; i++) {
        let reader = new FileReader();
        reader.readAsDataURL(e.target.files[i]);
        reader.onload = function () {
          self.imageUrls.push({
            id: self.img_id,
            file: reader.result,
          });
          self.img_id++;
        };
      }
      console.log("imageUrls: ", self.imageUrls);
    },
    async convertUploadedFilesToBase64(savedImages) {
            console.log(savedImages);
            alert()
            let self = this;
            for (let i = 0; i < savedImages.length; i++) {
                fetch(savedImages[i])
                    .then(res => res.blob())
                    .then(blob => {
                        let reader = new FileReader();
                        reader.readAsDataURL(blob);
                        reader.onloadend = function () {
                            let base64String = reader.result;
                            self.imageUrls.push({
                                id: 'new_' + self.img_id,
                                name: 'new',
                                file: base64String,
                            });
                            console.log('Base64 String - ', base64String);
                            console.log('Base64 String without Tags- ', base64String.substr(base64String.indexOf(', ') + 1));
                        }
                    });
            }
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
