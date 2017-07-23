<template>
	<div class="container">
			<div class="row padding-2">
				<div class="col-6">
					<picture-input 
			      ref="pictureInput" 
			      @change="onChange" 
			      width="300" 
			      height="376" 
			      margin="16" 
			      id = "picture"
			      accept="image/jpeg,image/png" 
			      size="10" 
			      :customStrings="{
			        upload: '<h1>Bummer!</h1>',
			        drag: 'Drag a 😺 GIF or GTFO'
			      }"
			      :removable="true">
		    	</picture-input>
				</div>
				<div class="col-6 flex-center">
		    	<img v-if="showGIF" class="loading-gif" src="/images/loading.gif" alt="Loading">
					<textarea class="text-area" id="readText" rows="21" cols="50">
			  	</textarea>
				</div>
			</div>
	</div>
</template>

<script>
	import Tesseract from 'tesseract.js';
	import PictureInput from 'vue-picture-input';
	//import CssCircle from 'v-circle/components/css-circle.vue';


	export default {
	  data () {
	    return {
	      showGIF: false
	    }
	  },
	  components: {
	  	PictureInput,
	  	//circleCss: CssCircle
	  },
	  methods: {
	    increment () {
	      this.count ++;
	    },
	    onChange () {
	    	this.showGIF = true;
	      if (this.$refs.pictureInput.image) {
	      	let image = this.$refs.pictureInput.image;
	      	let progress;
	      	Tesseract.recognize(image)
					.progress(message => {
						this.progressVal = message.progress;
					})
			    .catch(err => console.error(err))
			    .then(result => {
			    		document.getElementById('readText').innerHTML = result.text;
			    		this.showGIF = false;
			    });
	      } else {
	        console.log('FileReader API not supported: use the <form>, Luke!')
	      }
			}
	  }
	}
</script>

<style>
	html {
    box-sizing: border-box;
  }
  *, *:before, *:after {
    box-sizing: inherit;
  }
  * {
    padding: 0;
    margin: 0;
    position: relative;
  }
  body {
    font-family: -apple-system, BlinkMacSystemFont, sans-serif;
    font-weight: 300;
    background: #eee;
    min-height: 100vh;
    color: #fff;
    text-shadow: rgba(0, 0, 0, 0.25) 0 -1px 0;
    min-width: 615px;
  }

  .header {
  	background-color: #2F5FA5;
  	padding: 15px;
  }

  .footer {
  	background-color: #2F5FA5;
  	padding: 15px;
  	position:absolute;
   	bottom:0;
   	width: 100%;
  }

  .text-area {
	  -moz-border-bottom-colors: none;
	  -moz-border-left-colors: none;
	  -moz-border-right-colors: none;
	  -moz-border-top-colors: none;
	  border-color: -moz-use-text-color #FFFFFF #FFFFFF -moz-use-text-color;
	  border-image: none;
	  border-radius: 6px;
	  border-style: none solid solid none;
	  border-width: medium 1px 1px medium;
	  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.12) inset;
	  color: #555555;
	  /*font-family: "Helvetica Neue",Helvetica,Arial,sans-serif;*/
	  font-size: 1em;
	  line-height: 1.4em;
	  transition: background-color 0.2s ease 0s;
	  padding-left: 10px;
	}

textarea:focus {
    background: none repeat scroll 0 0 #FFFFFF;
    outline-width: 0;
}

.container {
	max-width:1140px;
	margin:0 auto;
	padding:0 1rem;
}

.row:before, 
.row:after {
  content:"";
  display: table ;
 	clear:both;
}

.col-3{ width: 25%;}
.col-6{ width: 50%;}

.col-3,.col-4,.col-6{
	float:left
}

.title{
	padding: 15px;
}

.progress-container{
	background: #fff;
}

.padding-1{
	padding: 1rem;
}

.padding-2{
	padding: 2rem;
}

.flex-center{
  display: flex;
  justify-content: center;
  align-items: center;
}

.loading-gif{
	z-index: 1; 
	position: absolute;
}

.btn {
    color: #fff;
    background-color: #337ab7;
    border-color: #2e6da4;
    display: inline-block;
    padding: 6px 12px;
    margin-bottom: 0;
    font-size: 14px;
    font-weight: 400;
    line-height: 1.42857143;
    text-align: center;
    white-space: nowrap;
    vertical-align: middle;
    -ms-touch-action: manipulation;
    touch-action: manipulation;
    cursor: pointer;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    background-image: none;
    border: 1px solid transparent;
    border-radius: 4px;
}

.copyright {
  font-size: smaller;
  text-align: center
}
</style>