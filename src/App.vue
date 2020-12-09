<template>
  <div id="app">
    
    <i-layout>
      <i-layout-header class="_padding-0">
        <i-navbar :collapse = false>
          <i-navbar-brand>
            <span class="">
              <svg xmlns="http://www.w3.org/2000/svg" class="theme_icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" />
              </svg>
            </span> 
            <span class="logo">ZAP</span>
          </i-navbar-brand>
          <i-nav class="">
              <!-- <i-nav-item>
                <a href="" class="">
                  
                  Home
                </a>
              </i-nav-item>
               -->
              <i-nav-item v-if="hideIntro">
                <label for="select_file_navbar" class=" _display-flex _align-items-center _cursor-pointer">
                  <svg xmlns="http://www.w3.org/2000/svg" style="color: dodgerblue; height: 24px; width: 24px; margin-right: 3px;" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" />
                  </svg>
                  Upload
                </label>
                <input v-show="false" type="file" accept=".md" id="select_file_navbar" ref="mdFile" @change="selectedFile">
              </i-nav-item>

          </i-nav>
        </i-navbar>
      </i-layout-header>
      
      <i-layout-content class="app-section">
          <i-container>
            <i-row v-if="!hideIntro" class="_display-flex _align-items-center">
              <i-column xl="5" lg="6" md="6" sm="6" xs="12">
                <h3 class="intro-heading">Preview Markdown files with ease</h3>
                <p class="_text-gray-60">Simply upload a markdown file to view it or Use our editor to write markdown and see its preview.</p>
              
                <label for="select_file" class="btn-upload _margin-top-1-2 _margin-bottom-1 _margin-right-1">Upload file</label>
                <button class="btn-noborder" @click="showPreview = true, hideIntro = true">Use Editor</button>
                <input v-show="false" type="file" id="select_file" ref="mdFile" @change="selectedFile">
              </i-column>

              <i-column xl="7" lg="6" md="6" sm="6" xs="12">
                <img src="https://picsum.photos/700" alt="Sample Preview" class="image -responsive">
              </i-column>
            </i-row>
            
            <i-row v-if="showPreview" class="_display-flex _align-items-stretch _flex-direction-sm-row">
              <!-- top row -->
              <i-column xs="12" class="_display-flex _align-items-center _padding-y-1 _margin-bottom-1">
                <!-- document name -->
                <i-column xs="8" class="_padding-left-0">
                  
                  <div class="">
                    <label for="file_name" class="_margin-y-1-4 _text-gray-50">Document name</label> <br>
                    <div class="_display-flex">
                      <input type="text" v-model="filename" name="file_name" id="file_name" class="save_filename">
                      <a v-if="mdtext != ''" href="javascript:void(0);" @click="saveFile" class="_margin-left-1 _display-flex _align-items-center">
                        <svg xmlns="http://www.w3.org/2000/svg" style="color: dodgerblue; height: 20px; width: 20px; margin-right: 3px;" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7H5a2 2 0 00-2 2v9a2 2 0 002 2h14a2 2 0 002-2V9a2 2 0 00-2-2h-3m-1 4l-3 3m0 0l-3-3m3 3V4" />
                        </svg>
                        Download
                      </a>
                    </div>
                  </div>

                  
                </i-column>

                <!-- document word count -->
                <i-column xs="4" class="text-counter _text-right">
                  Words: {{word_count}} <br>
                  Characters: {{character_count}}
                </i-column>
              </i-column>

              <i-column sm="6">
                <label for="md_text" class="_margin-0 _text-gray-50">
                  <small class="_margin-bottom-0 _padding-0">Your Text</small>
                </label>
              </i-column>

              <i-column sm="6">
                <small for="" class="_margin-bottom-0 _padding-0 _text-gray-50">Markdown Preview</small>
              </i-column>              

              <i-column sm="12" class="_padding-0 _display-flex">                
                <!-- editor -->
                <i-column sm="6" class="">
                  <i-textarea id="md_text" class="_width-100" v-model="mdtext" placeholder="Enter your Markdown text here." rows="15"></i-textarea>
                </i-column>

                <!-- preview box -->
                <i-column sm="6" class="">
                  <div class="preview-box _height-100">
                    <vue-markdown v-if="mdtext != ''" :source="mdtext" class=""></vue-markdown>

                    <div class="no-preview _height-100 _justify-content-center _display-flex _flex-direction-column _align-items-center" v-if="mdtext == ''">
                      <p class="no-preview-emote">(•ิ_•ิ)?</p>
                      <p class="_text-gray-60">Add markdown in editor or upload file to preview</p>
                    </div>
                  </div>
                  
                </i-column>
              </i-column>

              <!-- <i-column sm="6" class="_display-flex _flex-direction-column">
                
              </i-column> -->
            </i-row>

          </i-container>          
      </i-layout-content>
      
      <i-layout-footer class="_background-gray-20 _margin-top-2">
        <div class="_text-center _padding-y-1-4">
          Made by Akshay.
        </div>
      </i-layout-footer>
    </i-layout>
  
  </div>
</template>

<script>
import VueMarkdown from 'vue-markdown'

export default {
  name: 'App',
  data(){
    return{
      mdtext: '',
      word_count: 0,
      character_count: 0,
      filename: 'Untitled.md',
      fileExtension: 'md',
      fileUploaded: false,
      hideIntro: false,
      showPreview: false
    }
  },
  watch: {
    mdtext: function(newvalue){
      this.character_count = newvalue.length;
      // string[] text = newvalue;
      if(newvalue != ''){
        const regexp = /\s/;
        this.word_count = newvalue.split(regexp).length;
      }
      else{
        this.word_count = 0;
      }
    }
  },
  components: {
    VueMarkdown
  },
  methods: {
    wordCount(){
      console.log("Call word count");
    },
    selectedFile() {
      console.log('selected a file');
      console.log(this.$refs.mdFile.files[0]);
      
      let file = this.$refs.mdFile.files[0];
      // get extension of uploaded file
      let fileExtension = file.name.split('.').pop();
      
      if(!file || fileExtension !== 'md') {
        console.log("Invalid FILE: ", file.name);
        this.showPreview = false;
        return;
      }
      
      // if valid md file, read the contents and display
      let reader = new FileReader();
      reader.readAsText(file, "UTF-8");
      reader.onload =  evt => {
        this.mdtext = evt.target.result;
        this.showPreview = true;
      }
      reader.onerror = evt => {
        console.error(evt);
      }
      
    },

    saveFile(){
      const downloadToFile = (content, filename, contentType) => {
        const downloadlink = document.createElement('a');
        const file = new Blob([content], {type: contentType});
        
        downloadlink.href= URL.createObjectURL(file);
        downloadlink.download = filename;
        downloadlink.click();

        URL.revokeObjectURL(downloadlink.href);
      };

      if(this.mdtext != ''){
        let saveFileName = this.filename + '.'+ this.fileExtension;
        downloadToFile(this.mdtext, saveFileName, 'text/plain');
      }
      else{
        console.log("error: trying to save empty file");
      }
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono&family=Poppins&family=Ranchers&display=swap');/* font-family: 'Bowlby One SC', cursive;
// font-family: 'JetBrains Mono', monospace;
// font-family: 'Poppins', sans-serif; */


$gray100 : #f3f4f6;
$gray200 : #e5e7eb;
$gray400 : #9ca3af;
$gray700 : #374151;

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-family: 'Poppins', sans-serif;
  height: 100%;
  color: #333;

  .app-section{
    min-height: calc(100vh - 150px);
  }

  .logo{
    font-family: 'Ranchers', cursive;
    font-size: 1.75rem;
    line-height: 1
    }

  h1, h2, h3, h4, h5{
    font-family: 'Poppins', sans-serif;
    font-weight: 500;
    margin-top: 1rem;
  }

  .intro-heading{
    font-family: 'Jetbrains Mono';
    letter-spacing: 1px;
    line-height: 2rem;
    font-weight: bold;
    color: $gray700;
  }

  #md_text{
    font-family: 'JetBrains Mono', monospace;
  }

  .btn-upload{
    padding: 6px 12px;
    cursor: pointer;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  .btn-noborder{
    padding: 6px 12px;
    cursor: pointer;
    border:none;
    background: transparent;

    &:hover{
      color: orange;
    }

    &:focus{
      outline:none;
      color: blue;
    }
  }
  // input for filename
  .save_filename{
    width:200px; 
    font-size: 20px;
    letter-spacing: 1px;
    border-bottom:1px solid #eee;
    border-top: none;
    border-left: none;
    border-right: none;

    &:focus{
      border-bottom: #ccc 1px solid;
      outline: none;
    }
  }


  .theme_icon{
    color: black;
    height: 28px;
    width: 28px;
  }
  .preview-box{
    border: 1px dashed #ccc;
    padding-left: 10px;
    word-wrap: break-word;
    // height: 380px; 
    overflow: auto;
    // width: 100%;
    /* height: 15rem; */
  }

  .no-preview-emote{
    color: $gray200;
    font-size: 70px;
    line-height: 1px;
  }
}

</style>