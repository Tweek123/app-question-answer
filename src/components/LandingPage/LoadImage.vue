<template>
    <div ref="elnt">
            <div class="input-image-wrapper">
                <input class ="input-image-load" type="file" @change="previewImage" accept="image/*">
            </div>
        <div v-if="imageData.length > 0" class="image-preview">
            <img  class="preview" :src="imageData">
        </div>
    </div>
</template>
  <script>
      export default {
        props: {
        ev: Boolean,
        id: Number
      },
          data() {
              return {
        
                  inputShow: true,
                  imageData: ""  // we will store base64 format of image in this string
              }
          },
          methods: {
              previewImage: function (event) {
                  // Reference to the DOM input element
                  this.inputShow = false;
                  var input = event.target;
                  // Ensure that you have a file before attempting to read it
                  if (input.files && input.files[0]) {
                      // create a new FileReader to read this image and convert to base64 format
                      var reader = new FileReader();
                      // Define a callback function to run, when FileReader finishes its job
                      reader.onload = (e) => {
                          // Note: arrow function used here, so that "this.imageData" refers to the imageData of Vue component
                          // Read image as base64 and set to imageData
                          this.imageData = e.target.result;
                      }
                      // Start the reader job - read file as a data url (base64 format)
                      reader.readAsDataURL(input.files[0]);
                      this.$refs.elnt.children[0].style.display = 'none';
                  } 
              }
          },
            watch: {
          ev: function() {
              if(this.$refs.elnt.children[0] === undefined)
              {
                console.log(this.$refs.elnt.children[0]);
                this.$emit('delete-image', this.id);
              }
       }
    }
      }
  </script>

<style>
    .file-upload-form, .image-preview {
        font-family: "Helvetica Neue",Helvetica,Arial,sans-serif;
        padding: 20px;
    }
    img.preview {
        width: 80%;
        background-color: white;
        border: 1px solid #DDD;
        padding: 5px;
    }
    .input-image-wrapper {
        position: relative;
        background: url(../../img/galeria.png) no-repeat;
        background-size: cover;
        width: 30px;
        height: 30px;
        font-size: 0;
    }
    .input-image-load {
        height: 100%;
        width: 100%;
        z-index: 2;
        opacity: 0;
        font-size: 0;
        font-display: none;
        cursor: pointer;

    }
</style>
