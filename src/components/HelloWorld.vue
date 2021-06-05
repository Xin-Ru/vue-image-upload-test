<template>
  <div>
    <div>
      <label for="file-input" class="upload-button">+上傳圖片</label>
      <button type="button" @click="removeImages">移除所有照片</button>
    </div>
    <div class="upload-image-wrapper"
         :class="{'upload-image__active-border':isDragging}"
         v-show="!images.length"
         @dragover="dragover"
         @dragleave="dragleave">
      <div class="upload-image-wrapper-label">點擊上傳或拖曳至此</div>
      <input class="upload-input"
             type="file"
             accept="image/*"
             multiple="multiple"
             @change="uploadImage($event)"
             id="file-input">
    </div>
    <template v-if="images.length">
      <div class="image-preview">
        <div class="image-preview-item" v-for="(item, index) in images">
          <img :src="item" alt="image">
          {{ nameList[index] }}
        </div>
        <div class="image-preview-item-add-block"
             :class="{'upload-image__active-border':isDragging}"
             @dragover="dragover"
             @dragleave="dragleave">
          <input class="upload-input"
                 type="file"
                 accept="image/*"
                 multiple="multiple"
                 id="file-input-item"
                 @change="uploadImage($event)">
        </div>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      images: [],
      nameList: [],
      isDragging: false
    }
  },
  methods: {
    dragover(event) {
      event.preventDefault();

      const vm = this;
      vm.isDragging = true;
    },
    dragleave() {
      const vm = this;
      vm.isDragging = false;
    },
    uploadImage(event) {
      const vm = this;
      vm.isDragging = false;

      let files = event.target.files;
      console.log("event", event);

      for (let i = 0;i < files.length; i++) {
        const reader = new FileReader();
        reader.onload = (e) => {
          vm.images.push(e.target.result);
        }
        reader.readAsDataURL(files[i]);
        vm.nameList.push(files[i].name);
      }
    },
    uploadFile() {
      const form = new FormData();
      form.append(this.file, this.file.name);

      // axios.post('/upload', form);
    },
    removeImages() {
      this.images = [];
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.upload-button {
  font-size: 14px;
  line-height: 1.5;
  font-weight: bold;
  text-decoration: underline;
}

.delete-button {
  border: none;
  color: black;
  background-color: white;
}

.upload-image-wrapper {
  display:flex;
  justify-content: center;
  align-items: center;
  width: 90%;
  margin: 20px auto 0 auto;
  border: 2px dashed lightgray;
  border-radius: 0.6em;
  height: 150px;
  position: relative;
}

.upload-image-wrapper-label {
  width: 100%;
  position: absolute;
  top: 45%;
  text-align: center;
  color: black;
  font-size: 14px;
}

.upload-input {
  width: 100%;
  height: 100%;
  opacity: 0;
}

.image-preview {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 20px 20px;
  width: 90%;
  margin: 20px auto 0 auto;
}

.image-preview-item {
  width: 100%;

  img {
    width: 100%;
  }
}

.image-preview-item-add-block {
  width: 100%;
  height: auto;
  min-height: 100px;
  border-radius: 0.6em;
  border: 2px dashed lightgrey;
}

.image-preview-item-add-label {
  display: block;
  width: 100%;
  height: 100%;
}

.upload-image__active-border {
  border: 2px dashed black;
}
</style>
