<template>
  <div class="container">
    <h1 class="text-center mt-3 mb-4">{{ msg }}</h1>
    <div class="text-center">
      <b-button @click="showFilesModal" class="upload-start">Upload Modal</b-button>
    </div>
      <!-- Modal Component -->
      <b-modal ref="filesModal" id="modal-files-center" title="Files" size="lg" centered  @ok="filesModalOk" hide-footer>
          <b-tabs>
              <b-tab active class="first-tab">
                  <template slot="title">
                      <span class="title-content new-file-icon">Upload new file</span>
                  </template>
                  <div class="large-12 medium-12 small-12 cell">
                      <label>
                          <input type="file" id="files" ref="files" multiple v-on:change="handleFilesUpload()"/>
                      </label>
                  </div>
                  <div v-if="files.length === 0">
                      <div class="upload-content d-flex justify-content-center align-items-center">
                          <p>No previously uploaded files</p>
                      </div>
                      <div class="tab-footer">
                          <button class="btn upload-btn" v-on:click="addFiles()">Upload file</button>
                      </div>
                  </div>
                  <!--after upload-->
                  <div v-if="files.length !== 0">
                      <div class="upload-content">
                          <ul class="list-unstyled">
                              <b-media v-for="(file, key) in files" class="file-listing" :key="file">
                                  <section class="section-preview d-flex" v-bind:ref="'previewImage'+parseInt( key )"/>
                                  <section class="section-preview d-flex" v-bind:ref="'previewFile'+parseInt( key )" />
                                  <i class="delete-icon" v-on:click="removeFile( key )"></i>
                              </b-media>
                          </ul>
                      </div>
                      <div class="tab-footer">
                          <button class="btn upload-btn" v-on:click="addFiles()">Upload new file</button>
                      </div>
                  </div>
              </b-tab>
              <b-tab class="second-tab">
                  <template slot="title">
                      <span class="title-content link-icon">Upload from link</span>
                  </template>
                  <div class="upload-content d-flex justify-content-center align-items-center">
                      <b-form class="w-100">
                          <b-form-group id="link"
                                        label="File Link"
                                        label-for="link"
                                        description="">
                              <b-form-input id="fileLink"
                                            type="text"
                                            v-model="fileLink"
                                            @click="uploadFileByLink"
                                            required>
                              </b-form-input>
                          </b-form-group>
                      </b-form>
                  </div>
                  <div class="tab-footer d-flex">
                      <button class="btn upload-btn cancel mr-1">Cancel</button>
                      <button class="btn upload-btn ml-1">Upload new file</button>
                  </div>
              </b-tab>
              <b-tab class="third-tab">
                  <template slot="title">
                      <span class="title-content free-image-icon">Free images</span>
                  </template>
                  <!--before image upload-->
                  <div>
                      <div class="upload-content d-flex justify-content-center align-items-center">
                          <p>Upload free images</p>
                      </div>

                  </div>
                  <!--after image upload-->
                  <div>
                      <div class="upload-content">
                          <form class="search-free-image my-2 my-lg-0">
                              <input class="form-control" type="search" placeholder="Search image "
                                     aria-label="Search">
                              <i class="search-icon"></i>
                          </form>
                          <div class="free-images d-flex mt-4">
                              <div class="image-item">
                                  <img src="https://picsum.photos/125/125/?image=58" alt="">
                              </div>
                              <div class="image-item">
                                  <img src="https://picsum.photos/125/125/?image=58" alt="">
                              </div>
                              <div class="image-item">
                                  <img src="https://picsum.photos/125/125/?image=58" alt="">
                              </div>
                              <div class="image-item">
                                  <img src="https://picsum.photos/125/125/?image=58" alt="">
                              </div>
                              <div class="image-item ">
                                  <img src="https://picsum.photos/125/125/?image=58" alt="">
                              </div>
                              <div class="image-item ">
                                  <img src="https://picsum.photos/125/125/?image=58" alt="">
                              </div>
                              <div class="image-item ">
                                  <img src="https://picsum.photos/125/125/?image=58" alt="">
                              </div>
                              <div class="image-item ">
                                  <img src="https://picsum.photos/125/125/?image=58" alt="">
                              </div>
                              <div class="image-item ">
                                  <img src="https://picsum.photos/125/125/?image=58" alt="">
                              </div>
                              <div class="image-item ">
                                  <img src="https://picsum.photos/125/125/?image=58" alt="">
                              </div>
                              <div class="image-item ">
                                  <img src="https://picsum.photos/125/125/?image=58" alt="">
                              </div>
                              <div class="image-item ">
                                  <img src="https://picsum.photos/125/125/?image=58" alt="">
                              </div>
                              <div class="image-item ">
                                  <img src="https://picsum.photos/125/125/?image=58" alt="">
                              </div>
                              <div class="image-item ">
                                  <img src="https://picsum.photos/125/125/?image=58" alt="">
                              </div>
                              <div class="image-item ">
                                  <img src="https://picsum.photos/125/125/?image=58" alt="">
                              </div>

                          </div>
                      </div>
                      <div class="tab-footer d-flex">
                          <button class="btn upload-btn cancel mr-1">Cancel</button>
                          <button class="btn upload-btn ml-1">Upload new file</button>
                      </div>
                  </div>
              </b-tab>
          </b-tabs>
      </b-modal>
  </div>
</template>

<script>
export default {
  name: 'Dashboard',
  props: {
    msg: String
  },
  data () {
    return {
      files: [],
      fileLink: ''
    }
  },
  methods: {
    formatBytes (bytes, decimals) {
      if (bytes === 0) {
        return '0 Bytes'
      }
      const k = 1024
      const dm = decimals || 2
      const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB']
      const i = Math.floor(Math.log(bytes) / Math.log(k))
      return parseFloat((bytes / Math.pow(k, i)).toFixed(dm)) + ' ' + sizes[i]
    },
    showFilesModal () {
      this.$refs.filesModal.show()
    },
    hideFilesModal () {
      this.$refs.filesModal.hide()
    },
    filesModalOk (evt) {
      // Prevent modal from closing
      evt.preventDefault()
    },
    /*
       Adds a file
     */
    addFiles () {
      this.$refs.files.click()
    },
    /*
    Handles the uploading of files
   */
    handleFilesUpload () {
      let uploadedFiles = this.$refs.files.files

      /*
        Adds the uploaded file to the files array
      */
      for (var i = 0; i < uploadedFiles.length; i++) {
        this.files.push(uploadedFiles[i])
      }

      /*
        Generate file previews for the uploaded files
      */
      this.getPreviews()
    },
    /*
        Gets the preview image for the file.
    */
    getPreviews () {
      /*
        Iterate over all of the files and generate an image preview for each one.
      */
      for (let i = 0; i < this.files.length; i++) {
        let fileName = this.files[i].name
        let fileSize = this.formatBytes(this.files[i].size)

        /*
          Ensure the file is an image file
        */
        if (this.isImage(fileName)) {
          /*
            Create a new FileReader object
          */
          let reader = new FileReader()

          /*
            Add an event listener for when the file has been loaded
            to update the src on the file preview.
          */
          reader.addEventListener('load', function () {
            this.$refs['previewImage' + parseInt(i)][0].innerHTML = `<div class="img-box"><img src="${reader.result}" alt="${fileName}" /></div>
                                 <div><h5 class="mt-0 mb-1">${fileName}</h5>
                                  ${fileSize}</div>`
          }.bind(this), false)

          /*
            Read the data for the file in through the reader. When it has
            been loaded, we listen to the event propagated and set the image
            src to what was loaded from the reader.
          */
          reader.readAsDataURL(this.files[i])
        } else if (/\.(xml|pdf|docx?|xlsx?)$/i.test(this.files[i].name)) {
          setTimeout(() => {
            let fileIcon = '/images/file.png'
            if (this.isPdf(fileName)) {
              fileIcon = '/images/pdf.png'
            } else if (this.isDoc(fileName)) {
              fileIcon = '/images/doc.png'
            } else if (this.isExcel(fileName)) {
              fileIcon = '/images/xls.png'
            } else if (this.isXml(fileName)) {
              fileIcon = '/images/xml.png'
            }
            this.$refs['previewFile' + parseInt(i)][0].innerHTML = `<div class="img-box"><img src="${fileIcon}" alt="${fileName}" /></div>
                                 <div><h5 class="mt-0 mb-1">${fileName}</h5>
                                  ${fileSize}</div>`
          }, 200)
        }
      }
    },
    removeFile (key) {
      this.files.splice(key, 1)
    },
    isImage (fileName) {
      return /\.(jpe?g|png|gif)$/i.test(fileName)
    },
    isPdf (fileName) {
      return /\.(pdf)$/i.test(fileName)
    },
    isExcel (fileName) {
      return /\.(xlsx?)$/i.test(fileName)
    },
    isDoc (fileName) {
      return /\.(docx?)$/i.test(fileName)
    },
    isXml (fileName) {
      return /\.(xml)$/i.test(fileName)
    },
    uploadFileByLink () {
      console.log(this.fileLink)
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    input[type="file"]{
        position: absolute;
        top: -500px;
    }
    @font-face {
        font-family: 'Inter UI';
        font-style: normal;
        font-weight: 400;
        src: url('../assets/fonts/Inter-UI-Regular.woff2') format("woff2"),
        url(../assets/fonts/Inter-UI-Regular.woff) format("woff");
    }

    .modal-content {
        border: none !important;
    }

    .modal-header {
        background-color: #47A3FF;
    }

    h5.modal-title {
        color: #fff;
    }

    button.close {
        outline: none;
        color: #fff;
        opacity: 1;
    }

    .modal-content .modal-body {
        padding: 0;
    }

    .nav.nav-tabs .nav-item {
        flex: 1 1 33.33%;
        border-right: 1px solid #E4EDF9;
        margin-bottom: auto;
        padding: 10px;
        background-color: #F5FAFF;
    }

    .nav.nav-tabs .nav-item:last-child {
        border-right: none;
    }

    .nav-tabs .nav-link {
        border: none !important;
        padding: 0;
        text-align: center;
    }

    .modal-content .nav-tabs .nav-link.active, .modal-content .nav-tabs .nav-item.show .nav-link {
        border-color: unset;
        background-color: transparent;
    }

    .modal-body .nav-tabs {
        border-bottom: none;
        outline: none;
    }

    .title-content {
        padding-left: 25px;
        position: relative;
        color: #8BA2BE;
        font-family: 'Inter UI', sans-serif;
        font-size: 12px;
        line-height: 12px;
    }

    .new-file-icon:before {
        content: "";
        position: absolute;
        display: inline-block;
        width: 18px;
        height: 18px;
        left: 0;
        top: 50%;
        -webkit-transform: translateY(-50%);
        transform: translateY(-50%);
        background-image: url('../assets/image/plus.png');
        background-size: contain;
        background-repeat: no-repeat;
    }
    .link-icon:before {
        content: "";
        position: absolute;
        display: inline-block;
        width: 20px;
        height: 10px;
        left: 0;
        top: 50%;
        -webkit-transform: translateY(-50%);
        transform: translateY(-50%);
        background-image: url('../assets/image/link.png');
        background-size: contain;
        background-repeat: no-repeat;
    }
    .free-image-icon:before {
        content: "";
        position: absolute;
        display: inline-block;
        width: 20px;
        height: 16px;
        left: 0;
        top: 50%;
        -webkit-transform: translateY(-50%);
        transform: translateY(-50%);
        background-image: url('../assets/image/img.png');
        background-size: contain;
        background-repeat: no-repeat;
    }
    .upload-content {
        padding: 16px;
        min-height: 260px;
    }

    .upload-content p {
        padding: 16px;
        font-family: 'Inter UI', sans-serif;
        color: #8BA2BE;
        font-size: 12px;
        line-height: 21px;
    }

    .tab-footer {
        padding: 16px;
        border-top: 1px solid #F5FAFF;
    }

    .tab-footer .upload-btn {
        width: 100%;
        color: #FFFFFF;
        padding: 12px 5px;
        font-family: 'Inter UI', sans-serif;
        font-size: 11px;
        font-weight: bold;
        line-height: 15px;
        border-radius: 2px;
        -webkit-border-radius: 2px;
        -moz-border-radius: 2px;
        background-color: #1A8CFF;
        text-transform: uppercase;
    }

    /*first tab*/
    .first-tab .upload-content {
        padding: 0;
    }

    .upload-content .media {
        position: relative;
        padding: 8px 16px;
        border-bottom: 1px solid #F5FAFF;
        -webkit-transition: all ease .3s;
        transition: all ease .3s;
    }

    .upload-content .media:last-child {
        border-bottom: none;
    }

    .upload-content .media i.delete-icon {
        position: absolute;
        right: 20px;
        top: 50%;
        display: inline-block;
        width: 12px;
        height: 16px;
        -webkit-transform: translateY(-50%);
        transform: translateY(-50%);
        -webkit-transition: all ease .3s;
        transition: all ease .3s;
        background-image: url('../assets/image/delete.png');
        background-size: contain;
        background-repeat: no-repeat;
        opacity: 0;
        cursor: pointer;
    }

    .upload-content .media:hover i {
        opacity: 1;
    }
    .upload-content .media .img-box {
        width: 32px;
        height: 32px;
        margin-right: 15px;
    }
    .upload-content .media .img-box  img{
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .upload-content .media-body h5 {
        font-family: 'Inter UI', sans-serif;
        color: #47A3FF;
        font-size: 12px;
        line-height: 12px;
    }

    .upload-content .media-body {
        font-family: 'Inter UI', sans-serif;
        color: #BECDE2;
        font-size: 12px;
        line-height: 12px;
    }

    /*second tab*/
    .tab-footer .btn.cancel {
        border: 1px solid #E7EBF2;
        background-color: #FFFFFF;
        color: #7C8EAA;
    }

    .second-tab .b-form-group label {
        font-family: 'Inter UI', sans-serif;
        color: #8BA2BE;
        font-size: 10px;
        line-height: 20px;
    }

    .second-tab .b-form-group input {
        padding: 14px 12px;
        font-family: 'Inter UI', sans-serif;
        color: #4D5968;
        font-size: 12px;
        line-height: 15px;
    }

    /*third tab*/

    .third-tab .search-free-image {
        position: relative;
    }

    .third-tab .search-free-image i.search-icon {
        position: absolute;
        right: 8px;
        top: 50%;
        -webkit-transform: translateY(-50%);
        transform: translateY(-50%);
        width: 16px;
        height: 16px;
        background-image: url('../assets/image/search.png');
        background-size: contain;
        background-repeat: no-repeat;
        cursor: pointer;
    }

    .third-tab .search-free-image input {
        padding-right: 30px;
        font-family: 'Inter UI', sans-serif;
        color: #8BA2BE;
        font-size: 12px;
        line-height: 12px;
    }

    ::-moz-placeholder {
        color: #8BA2BE;
        font-size: 12px;
        line-height: 12px;
        opacity: 0.3;
    }

    ::-webkit-input-placeholder {
        color: #8BA2BE;
        font-size: 12px;
        line-height: 12px;
        opacity: 0.3;
    }

    :-ms-input-placeholder {
        color: #8BA2BE;
        font-size: 12px;
        line-height: 12px;
        opacity: 0.3;
    }

    :-moz-placeholder {
        color: #8BA2BE;
        font-size: 12px;
        line-height: 12px;
        opacity: 0.3;
    }

    .free-images {
        flex-wrap: wrap;
    }
    .image-item {
        margin-right: 1%;
        margin-bottom: 1%;
        height: 95px;
        width: 19%;
    }
    .image-item:nth-child(5n) {
        margin-right: 0;
    }

    .image-item:hover {
        border: 3px solid #1A8CFF;
    }
    .image-item img {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }
</style>
