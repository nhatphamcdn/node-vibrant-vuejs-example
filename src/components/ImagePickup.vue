<template>
  <div class="image-pickup">
    <div class="links">
      <label class="image__upload--label" for="upload" title="Upload an image">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M11 3v2h-4v-2h4zm3 6l-2.519 4-2.481-1.96-4 5.96h14l-5-8zm-7.5 1c.828 0 1.5-.671 1.5-1.5 0-.828-.672-1.5-1.5-1.5s-1.5.672-1.5 1.5c0 .829.672 1.5 1.5 1.5zm10.5 9h-4v2h4v-2zm-6 2v-2h-4v2h4zm2-18v2h4v-2h-4zm6 0v2h3v3h2v-5h-5zm-17 2h3v-2h-5v5h2v-3zm20 14h-3v2h5v-5h-2v3zm0-5h2v-4h-2v4zm-20-4h-2v4h2v-4zm3 9h-3v-3h-2v5h5v-2z"/></svg>
        <input id="upload" class="image__upload" type="file" name="img" @change="uploadFile" />
      </label>
    </div>
    <div class="image__main" :style="{ backgroundImage: 'url(' + imageSrc + ')' }" :class="{isLoading: isLoading}">
      <span :class="{active: isLoading}" class="loading">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M14 22c0 1.104-.896 2-2 2s-2-.896-2-2 .896-2 2-2 2 .896 2 2zm-2-22c-1.104 0-2 .896-2 2s.896 2 2 2 2-.896 2-2-.896-2-2-2zm10 10c1.104 0 2 .896 2 2s-.896 2-2.001 2c-1.103 0-1.999-.895-1.999-2s.896-2 2-2zm-22 2c0 1.105.896 2 2 2s2-.895 2-2c0-1.104-.896-2-2-2s-2 .896-2 2zm19-9c1.104 0 2 .896 2 2s-.896 2-2.001 2c-1.103 0-1.999-.895-1.999-2s.896-2 2-2zm0 14c1.104 0 2 .896 2 2s-.896 2-2.001 2c-1.103 0-1.999-.895-1.999-2s.896-2 2-2zm-14-14c1.104 0 2 .896 2 2s-.896 2-2.001 2c-1.103 0-1.999-.895-1.999-2s.896-2 2-2zm0 14c1.104 0 2 .896 2 2s-.896 2-2.001 2c-1.103 0-1.999-.895-1.999-2s.896-2 2-2z"/></svg>
      </span>
    </div>
    
    <palette :palette="palette" v-if="palette.length" />
  </div>  
</template>

<script>
import * as Vibrant from 'node-vibrant';
import ntc from '@/plugins/ntc';

export default {
  components: {
    Palette: () => import('./Palette'),
  },
  data() {
    return {
      palette: [],
      imageSrc: '',
      isLoading: false,
    };
  },
  created() {
    ntc.init();
    this.randomImage()
  },
  methods: {
    randomImage() {
      const randomNumber = Math.floor(Math.random() * 633)
      this.isLoading = true
      fetch(`https://source.unsplash.com/collection/490175/500x250/?sig=${randomNumber}`)
        .then((response) => {
          this.getPalette(response.url)
          this.isLoading = false
        })
    },
    getPalette(imageSrc) {
      this.imageSrc = imageSrc

      Vibrant.from(imageSrc).maxColorCount(200).getPalette().then((palette) => {
        const colors = []
        let number = 0
        for(let color in palette) {
          number = number + 1
          const type = color
          const typeTextColor = palette[color].getTitleTextColor()
          const hex = palette[color].getHex()
          const hexTextColor = palette[color].getBodyTextColor()
          const name = ntc.name(hex)[1];
          const nameTextColor = palette[color].getBodyTextColor()
          colors.push({ number, type, typeTextColor, hex, hexTextColor, name, nameTextColor })
        }

        this.palette = colors
      });
    },
    uploadFile(event) {
      const file = event.target.files[0]
      const reader = new FileReader()
      reader.addEventListener("load", () => {
        this.getPalette(reader.result)
      }, false);
      if (file) {
        reader.readAsDataURL(file)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
label {
  cursor: pointer;
}

.app-wrapper {
  display: flex;
  justify-content: center;
}

.image-pickup {
  position: relative;
  margin: 4em auto;
  width: 100%;
  max-width: 500px;
  background: #fff;
  border-radius: 4px;
  overflow: hidden;
  box-shadow: 0 0 10px rgba(black, .25);
}

.image__main {
  position: relative;
  width: 100%;
  height: 250px;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  display: flex;
  justify-content: center;
  align-items: center;
  &.isLoading {
    &:before {
      position: absolute;
      top: 0;
      left: 0;
      content: '';
      width: 100%;
      height: 100%;
      display: block;
      background: rgba(white, .8);
    }
  }
}

.links {
  position: absolute;
  top: 0;
  right: 0;
  background: #fff;
  padding: 10px 15px 5px;
  border-radius: 0 0 0 4px;
  z-index: 10;
  svg {
    transition: opacity .3s ease;
    opacity: .5;
    &:hover {
      opacity: 1;
    }
  }
}

.image__new {
  padding: 0;
  margin-left: 7px;
  &[disabled], &[disabled]:hover {
    svg {
      opacity: .2;
    }
  }
}
.image__upload {
  position: absolute;
  opacity: 0;
  z-index: -1;
  width: 1px;
  height: 1px;
}

h1 {
  margin: 1.4em 0;
  font-size: 1.4em;
  letter-spacing: 2px;
  font-weight: 700;
  color: #333;
}

.loading {
  display: none;
  width: 50px;
  height: 50px;
  position: relative;
  &.active {
    display: block;
  }
  svg {
    position: absolute;
    animation: spin 4s linear infinite;
    width: 50px;
    height: 50px;
  }
}

@keyframes spin { 
  100% { 
    transform:rotate(360deg); 
  } 
}
</style>