<template>
  <div id="app">
    
    <i-layout>
      <i-layout-header class="_padding-0">
        <i-navbar :collapse = false class="_padding-0 app-navbar">
          <i-navbar-brand>
            <a href="javascript:void(0);" class="" @click="hideIntro = false, showPreview = false">
              <span class="">
                <svg xmlns="http://www.w3.org/2000/svg" class="svg_icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" />
                </svg>
              </span> 
              <span class="logo">ZAP</span>
            </a>
          </i-navbar-brand>
          <i-nav class="">
              <!-- <i-nav-item>
                <a href="" class="">
                  
                  Home
                </a>
              </i-nav-item>
               -->
              <i-nav-item v-if="hideIntro" style="margin-top: 4px;">
                <label for="select_file_navbar" class="btn-nav-upload _display-flex _align-items-center _cursor-pointer">
                  <svg xmlns="http://www.w3.org/2000/svg" style="color: gold; height: 24px; width: 24px; margin-right: 3px;" fill="none" viewBox="0 0 24 24" stroke="currentColor">
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
          <div class="app-intro" v-if="!hideIntro" >
            <div class="app-jumbotron  _text-center">
              <i-container>
                <h3 class="intro-heading">Preview Markdown files with ease</h3>
                <p class="_text-gray-50">Simply upload a markdown file to view it or Use our editor to write markdown and see its preview.</p>
              
                <label for="select_file" class="btn-upload _margin-top-1-2 _margin-bottom-1 _margin-right-1">Upload file</label>
                <i-tooltip variant="dark" size="sm">
                  <i-button class="btn-noborder" @click="showPreview = true, hideIntro = true">Use Editor</i-button>
                  <template slot="body">Write markdown in editor</template>
                </i-tooltip>

                <input v-show="false" type="file" id="select_file" ref="mdFile" accept=".md" @change="selectedFile">
              </i-container>
            </div>

            <i-container class="app-screenshot">
              <i-row class="_display-flex _align-items-center">
                <i-column sm="5" class="text-screenshot">
                  <h5 class="_padding-x-1-2">Markdown Text</h5>
                  <img src="./assets/text-screenshot.png" alt="Sample Preview" class="image -responsive">
                </i-column>

                <i-column class="preview-arrows _display-xs-none _display-sm-flex">
                  <svg xmlns="http://www.w3.org/2000/svg" style="color: rgb(235, 179, 233);" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 5l7 7-7 7M5 5l7 7-7 7" />
                  </svg>
                </i-column>
                
                <i-column sm="5" class=" preview-screenshot">
                  <h5 class="_padding-x-1-2">It's Preview</h5>
                  <img src="./assets/preview-screenshot.png" alt="Sample Preview" class="image -responsive">
                </i-column>
              </i-row>
            </i-container>
          </div>

          <i-container>
            <i-row v-if="showPreview" class="_display-flex _align-items-stretch _flex-direction-sm-row">
              <!-- top row -->
              <i-column xs="12" class="_display-flex _align-items-center _flex-wrap _padding-y-1 _margin-bottom-1">
                <!-- document name -->
                <i-column sm="8" class="_padding-left-0">
                  
                  <div class="-align-left">
                    <label for="file_name" class="_margin-y-1-4 _text-gray-50">Document name</label> <br>
                    <div class="_display-flex _flex-wrap">
                      <input type="text" v-model="filename" name="file_name" id="file_name" class="save_filename _margin-right-1 _margin-bottom-1">
                      <i-tooltip v-if="mdtext != ''" variant="dark" size="sm">
                        <a href="javascript:void(0);" @click="saveFile" class=" _display-flex _align-items-center">
                          <svg xmlns="http://www.w3.org/2000/svg" style="color: lightsalmon; height: 20px; width: 20px; margin-right: 3px;" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7H5a2 2 0 00-2 2v9a2 2 0 002 2h14a2 2 0 002-2V9a2 2 0 00-2-2h-3m-1 4l-3 3m0 0l-3-3m3 3V4" />
                          </svg>
                          Download
                        </a>
                        <template slot="body">Save markdown</template>
                      </i-tooltip>
                      
                      
                    </div>
                  </div>

                  
                </i-column>

                <!-- document word count -->
                <i-column sm="4" class="text-counter _text-right">
                  Words: {{word_count}} <br>
                  Characters: {{character_count}}
                </i-column>
              </i-column>

              <i-column xs="6">
                <label for="md_text" class="_margin-0 _text-gray-60">
                  <small class="_margin-bottom-0 _padding-0">Your Text</small>
                </label>
              </i-column>

              <i-column xs="6">
                <small for="" class="_margin-bottom-0 _padding-0 _text-gray-60">Markdown Preview</small>
              </i-column>              

              <i-column sm="12" class="_padding-0 _display-flex">                
                <!-- editor -->
                <i-column sm="6" class="">
                  <i-textarea id="md_text" class="_width-100" v-model="mdtext" placeholder="Enter your Markdown text here." rows="21"></i-textarea>
                </i-column>

                <!-- preview box -->
                <i-column sm="6" class="preview-col">
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
      
      <i-layout-footer class="app-footer _padding-top-1 _padding-bottom-1-2 _margin-top-4">
        <div class="custom-container">
          <div class="_display-flex _justify-content-space-between _padding-top-1-4 _padding-bottom-0">
            <div class="footer-buttons _display-flex _align-items-center">
              <!-- Place this tag where you want the button to render. -->
              <a class="github-button" href="https://github.com/akshay-rajput/vue_md_previewer/fork" 
                  data-icon="octicon-repo-forked" data-size="large" 
                  aria-label="Fork root-0/vue_md_previewer on GitHub">
                Github
              </a>
              
              <a href="https://www.linkedin.com/in/akshay-rajput/" class="btn-linkedin">
                <span>LinkedIn</span>
              <!-- <img src="./assets/linkedin.png" alt="Linkedin" style="height: 14px;"> -->
              </a>
            </div>
            <div class="footer-credits _text-gray-40">
              &lt;/&gt; by 
              <i-tooltip variant="dark" size="sm">
                <a href="https://akshayr.netlify.app/" target="_blank" rel="noopener noreferrer">Akshay</a>
                <template slot="body">Developer's website</template>
              </i-tooltip>
            </div>
          </div>
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

// $dark: #583d72;
// $light: #9f5f80;
// $accent: #ffba93;
// $accentDark: #ff8e71;
$try: rgb(235, 179, 233);

$dark: darkmagenta;
$orchid: orchid;
$accent: lightsalmon;
$offwhite: #fcfcfc;
// setup theme here
#app{
  background: $offwhite;
}
.app-jumbotron{
  background:$dark;
  padding: 45px;
}
.app-footer, .app-navbar{
  background: $dark !important;
}
.intro-heading{
  color: $orchid;
}
a{
  color:$accent !important;
  transition: all ease 0.35s;
  &:hover{
    color: $orchid !important;
  }
}

// other css
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-family: 'Poppins', sans-serif;
  height: 100%;
  color: #333;

  .app-section{
    min-height: calc(100vh - 120px);
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
    font-family: 'Jetbrains Mono', monospace;
    letter-spacing: 1px;
    line-height: 2rem;
    font-weight: bold;
  }

  #md_text{
    font-family: 'JetBrains Mono', monospace;
  }

  .btn-upload{
    padding: 6px 12px;
    cursor: pointer;
    border: 1px solid $orchid;
    background: $orchid;
    color: white !important;
    border-radius: 5px;
    transition: all ease 0.35s;

    &:hover{
      background: $accent;
    }
  }
  .btn-noborder{
    font-family: 'Jetbrains Mono', monospace;
    padding: 6px 12px;
    cursor: pointer;
    border:none;
    color: $accent;
    font-weight: 500;
    background: transparent;

    &:hover{
      color: gold;
    }

    &:focus{
      outline:none;
      color: $accent;
    }
  }
  .btn-nav-upload{
    color: gold;
    padding: 4px;

    &:hover{
      text-decoration: underline;
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


  .svg_icon{
    color: $accent;
    height: 28px;
    width: 28px;
    margin-top: 8px;
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

  .custom-container{
    padding-left: 15px;
    padding-right: 15px;
    margin-left: auto;
    margin-right: auto;;

    @media screen and (min-width: 0){
      max-width: 100%;
    }
    @media screen and (min-width: 576px){
      max-width: 550px;
    }
    @media screen and (min-width: 767px){
      max-width: 740px;
    }
    @media screen and (min-width: 992px) {
      max-width: 962px;
    }
    @media screen and (min-width: 1200px) {
      max-width: 1168px;
    }


    
    
  }

  .btn-linkedin{
    background: $orchid;
    font-family: -apple-system,BlinkMacSystemFont,Segoe UI,Helvetica,Arial,sans-serif;
    margin-top: -5px;
    margin-left: 15px;
    color:white !important;
    font-size: 12px;
    letter-spacing: 0.1px;
    padding: 4px 8px;
    border-radius: 4px;
    transition: all ease 0.35s;
    &:hover{
      text-decoration: none;
      // border-color: $accent;
      color: white;
      background: $accent;
    }
  }

  .footer-credits{
    font-family: 'Ranchers', cursive;
    letter-spacing: 1px;
  }
}

</style>
