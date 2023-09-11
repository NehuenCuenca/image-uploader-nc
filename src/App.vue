<template>
  <main>
    <div class="card" v-if="!isUploadingImage && !imageHasBeenSent">
      <span class="title">Upload your image</span>
      <span class="subtitle">File should be Jpeg, Png,...</span>
      <div class="draggable-zone" @dragover.prevent @drop="onDrop">
        <img src="./assets/placeholder-img.webp" alt="placeholder image" class="placeholder-img" v-if="!newImage">
        <img :src="newImage" alt="real image" class="placeholder-img" v-else>
        <span class="info-img">Drag & Drop your image here</span>
      </div>
      <span class="placeholder-text">Or</span>
      <div class="upload-file">
        <input type="file" accept="image/*" ref="fileInput" id="files" class="hidden" @change="onChange" />
        <label for="files">Choose a file</label>
      </div>
    </div>

    <div class="card loading" v-if="isUploadingImage && imageHasBeenSent">
      <span class="title">Uploading...</span>
      <div class="progess-bar">
        <div class="progress"></div>
      </div>
    </div>

    <template v-if="!isUploadingImage && imageHasBeenSent">
      <div class="card" v-if="imageHasBeenUpload">
        <i class="icon">✅</i>
        <span class="title">Uploaded Successfully!</span>
        <img :src="urlNewImage" alt="" class="original-img">
        <div class="link-img">
          <input type="text" disabled :value="urlNewImage">
          <button type="button" class="copy-clipboard" @click="copyToClipboard">Copy Link</button>
        </div>
      </div>

      <div class="card" v-if="!imageHasBeenUpload">
        <i class="icon">❌</i>
        <span class="title">Uploaded Failed!</span>
        <p>{{ msgError }}</p>
        <button class="try-again" @click="retryUpload">Try again!</button>
      </div>
    </template>
  </main>
  <footer>created by Nehuen - devChallenges.io</footer>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    // STATE
    const newImage = ref(null)
    const urlNewImage = ref('')
    const msgError = ref('')

    const imageHasBeenSent = ref(false)
    const isUploadingImage = ref(false)
    const imageHasBeenUpload = ref(false)


    // METHODS 
    const createFile = (file) => {
      if (!file.type.match('image.*')) {
        alert('Select an image');
        return;
      }

      let reader = new FileReader();

      reader.addEventListener('load', async function (e) {
        newImage.value = e.target.result;
        await uploadImage();
      })

      reader.readAsDataURL(file);
    }

    const uploadImage = async () => {
      imageHasBeenSent.value = true
      isUploadingImage.value = true

      // https://api.cloudinary.com/v1_1/de9d1foso/image/upload
      const preset = 'ml_default'
      const cloud_name = 'de9d1foso'

      const cloudinaryUrl = `https://api.cloudinary.com/v1_1/${cloud_name}/image/upload`

      const formData = new FormData();
      formData.append('upload_preset', `${preset}`)
      formData.append('file', newImage.value);

      try {
        const res = await fetch(cloudinaryUrl, {
          method: 'POST',
          body: formData,
        });


        isUploadingImage.value = false
        imageHasBeenUpload.value = res.ok
        console.log(imageHasBeenUpload.value);
        if (!res.ok) throw new Error(`${res.statusText}`)

        const data = await res.json();
        urlNewImage.value = data.secure_url
      } catch (error) {
        console.error(error);
        msgError.value = error.message
        alert(`${error}`)
      }
    }

    // https://codepen.io/raffo1234/pen/beQXwe
    const onDrop = (e) => {
      e.stopPropagation();
      e.preventDefault();
      console.log('ON DROP');

      var files = e.dataTransfer.files;
      createFile(files[0]);
    }

    const onChange = (e) => {
      console.log('ON CHANGE');
      const files = e.target.files
      createFile(files[0]);
    }

    const copyToClipboard = async () => {
      await navigator.clipboard.writeText(urlNewImage.value)
    }

    const retryUpload = () => {
      newImage.value = null
      urlNewImage.value = ''
      msgError.value = ''

      imageHasBeenSent.value = false
      isUploadingImage.value = false
      imageHasBeenUpload.value = false
    }

    return {
      // STATE
      newImage,
      urlNewImage,
      isUploadingImage,
      imageHasBeenSent,
      imageHasBeenUpload,
      msgError,

      // METHODS
      onDrop,
      onChange,
      copyToClipboard,
      retryUpload
    }
  }
}
</script>

<style scoped>
main {
  width: 100%;
  display: grid;
  place-items: center;
}

.card {
  box-shadow: 0px 4px 12px 0px #0000001A;
  border-radius: 12px;
  padding: 26px;
  width: 50%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 3vh 0;
}

.title {
  font: 500 1.125rem 'Poppins', sans-serif;
  color: #4F4F4F;
}

.subtitle {
  font: 500 10px 'Poppins', sans-serif;
  color: #828282;
}

.draggable-zone {
  width: 40%;
  /* border: 1px solid #97BEF4; */
  /* border-radius: 12px; */
  /* border-style: dashed; */
  background-color: #F6F8FB;
  /* https://kovart.github.io/dashed-border-generator/ */
  background-image: url("data:image/svg+xml,%3csvg width='100%25' height='100%25' xmlns='http://www.w3.org/2000/svg'%3e%3crect width='100%25' height='100%25' fill='none' stroke='%2397BEF4FF' stroke-width='3' stroke-dasharray='6%2c 14' stroke-dashoffset='0' stroke-linecap='square'/%3e%3c/svg%3e");
  padding: 2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 3vh 0;
}

.placeholder-img {
  width: 35%;
  border-radius: 5px;
}

.placeholder-text,
.info-img {
  font: 500 12px 'Poppins', sans-serif;
  color: #BDBDBD;
}

.upload-file label {
  cursor: pointer;
  border-radius: 8px;
  padding: 7px 10px;
  background-color: #2F80ED;
  font: 500 12px 'Noto Sans', sans-serif;
  color: white;
}

.loading :is(.title) {
  align-self: flex-start;
}

.progess-bar {
  position: relative;
  width: 90%;
  height: auto;
  background-color: #F2F2F2;
}

.progress {
  position: absolute;
  top: 0;
  left: 0;
  width: 30%;
  height: 5px;
  border-radius: 8px;
  background-color: #2F80ED;
}

.original-img {
  border-radius: 12px;
  width: 50%;
  height: 30vh;
  object-fit: contain;
}

.link-img {
  width: 50%;
  border-radius: 8px;
  border: 1px solid #E0E0E0;
  padding: 2px 4px;
  display: flex;
  justify-content: space-between;
}


.link-img input {
  width: 100%;
  border: none;
  outline: none;
  padding: 5px 5px;
  font: 500 12px 'Poppins', sans-serif;
}

button:is(.copy-clipboard, .try-again) {
  width: fit-content;
  white-space: nowrap;
  background-color: #2F80ED;
  color: white;
  font: 500 12px 'Poppins', sans-serif;
  border-radius: 8px;
  padding: .5rem .8rem;
}


footer {
  color: #A9A9A9;
  font: 500 14px 'Poppins', sans-serif;
  place-items: center;
}
</style>