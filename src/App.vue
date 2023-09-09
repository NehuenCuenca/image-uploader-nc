<template>
  <main>
    <div class="card">
      <span class="title">Upload your image</span>
      <span class="subtitle">File should be Jpeg, Png,...</span>
      <div class="draggable-zone">
        <img src="./assets/placeholder-img.webp" alt="placeholder image" class="placeholder-img">
        <span class="info-img">Drag & Drop your image here</span>
      </div>
      <span class="placeholder-text">Or</span>
      <button type="button" class="upload">Choose a file</button>
    </div>

    <!-- <div class="card loading">
      <span class="title">Uploading...</span>
      <div class="progess-bar">
        <div class="progress"></div>
      </div>
    </div> -->

    <!-- <div class="card">
      <i class="icon">✅</i>
      <span class="title">Uploaded Successfully!</span>
      <img src="" alt="" class="original-img">
      <div class="link-img">
        <input type="text" disabled value="https://images.yourdomain.com/photo-1496950866446-325...">
        <button type="button" class="copy-clipboard">Copy Link</button>
      </div>
    </div> -->
  </main>
  <footer>created by Nehuen - devChallenges.io</footer>
</template>

<script>

export default {
  setup() {

    return {
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

.card .upload {
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

.icon {}

.original-img {
  border-radius: 12px;
  width: 50%;
  height: 30vh;
  background-color: blueviolet;
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
button.copy-clipboard {
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
}</style>