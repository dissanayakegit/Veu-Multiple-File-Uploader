<template>
    <div class="form-group m-form__group row">
        <div class="col-lg-4">
            <div class="component-wrapper">
                <div class="image-previewer">
                    <div v-for="i in [currentIndex]" :key="i">
                        <img :src="currentImg"/>
                    </div>

                    <div class="navi-buttons m-demo-icon">
                        <div class="m-demo-icon__preview">
                            <i class="fa fa-angle-left" @click="previousImage()"></i>
                        </div>
                        <div class="m-demo-icon__preview">
                            <i class="fa fa-angle-right" @click="nextImage()"></i>
                        </div>
                    </div>
                </div>

                <div class="action-menu">
                    <div class="custom-file">
                        <input
                            id="file-upload"
                            ref="file"
                            class="custom-file-input"
                            multiple
                            name="document_file"
                            type="file"
                            @change="setFile($event)"
                        />
                        <label class="custom-file-label selected overflow-hide">{{ $t('Browse') }}</label>
                    </div>

                    <div class="col-lg-1 text-center">
                        {{currentImgId}}
                        <a
                            class="btn btn-outline-danger m-btn m-btn--icon m-btn--icon-only m-btn--pill fd-delete-btn"
                            :id="currentImgId"
                            @click="deleteFile(currentImgId)"
                        >
                            <i class="fas fa-trash"></i>
                        </a>
                    </div>
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
            img_id: 0,
            currentIndex: 0
        }
    },

    computed: {
        currentImg() {
            if (this.imageUrls.length > 0) {
                return this.imageUrls[Math.abs(this.currentIndex) % this.imageUrls.length].file;
            }
            return 0;
        },
        currentImgId() {
            if (this.imageUrls.length > 0) {
                return this.imageUrls[Math.abs(this.currentIndex) % this.imageUrls.length].id;
            }
            return 0;
        }
    },

    methods: {
        setFile(e) {
            let self = this;
            let reader = new FileReader();

            if(e.target.files.length > 0){
                for (let i = 0; i < e.target.files.length; i++) {
                     alert();
                    reader.readAsDataURL(e.target.files[i]);
                    reader.onload = function () {
                        self.imageUrls.push(
                            {
                                id: self.img_id,
                                file: reader.result
                            });
                        self.img_id++;
                    }
                }
                console.log('imageUrls: ', self.imageUrls);

            }
        },
        deleteFile(img_id){
            let self = this;

            // self.imageUrls = self.imageUrls.filter(function( obj ) {
            //     return obj.id !== img_id;
            // });
            // console.log(img_id, self.imageUrls);

            self.imageUrls.forEach(function (object){
                if(object.id == img_id){
                    self.imageUrls.splice(object, 1);
                }
            })
            console.log(img_id, self.imageUrls);


        },

        nextImage() {
            this.currentIndex += 1;
        },

        previousImage() {
            this.currentIndex -= 1;
        }
    }
}
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
