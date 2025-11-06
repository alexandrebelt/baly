<template>
  <div class="white-container">

    <div class="heading-in" v-if="!startedWidget">
      <img id="hand" src="../assets/images/hand.png" />
      <h6> Toque na <span>Bala</span> para ganhar a sua figurinha!</h6>
    </div>


    <div v-else class="sorteio-imagens " @click="stopShuffling">
      <div v-if="isShuffling" class="heading-in centralize">
        <h6> Toque na <span>Figurinha</span> para sortear uma!</h6>
      </div>
      <div v-else class="fig-title heading-in centralize">
        <h6>{{ stickerText }}</h6>
      </div>
      <div class="wrapper-figurinhas">
        <img class="imgs-figurinhas" v-if="selectedImage" :src="selectedImage" alt="Imagem sorteada" />
      </div>

      <div v-if="!isShuffling">
        <div class="share-text">
          <p v-html="stickerShareText"></p>
        </div>
        <a id="button-share" @click="shareSticker">
          <div class="btn">
            <img src="../assets/images/share.png" alt="share">
            <span>compartilhar</span>
          </div>
        </a>
      </div>
    </div>
  </div>



  <div id="lata-e-bala">
    <img id="lata" src="../assets/images/lata.png">
    <img id="bala" src="../assets//images/bala.png">
  </div>


  <div v-if="showShareOptions" class="locked-background" @click="closeShare">
    <div class="share-options">
      <h6>Compartilhe em suas redes!</h6>
      <div class="share-buttons">
        <a :href="'https://api.whatsapp.com/send?text=' + encodeURIComponent(texto + ' ' + url)" target="_blank">
          <img src="../assets/images/social/whatsapp.png" alt="whatsapp">
        </a>
        <a @click="copiaCola" href="https://www.instagram.com/direct/inbox/" target="_blank">
          <img src="../assets/images/social/instagram.png" alt="Instagram">
        </a>

        <a @click="copiaCola" :href="'https://m.me/?link=' + encodeURIComponent(url)" target="_blank">
          <img src="../assets/images/social/messenger.png" alt="messenger">
        </a>


        <a :href="'https://twitter.com/intent/tweet?text=' + encodeURIComponent(texto) + '&url=' + encodeURIComponent(url)"
          target="_blank">
          <img src="../assets/images/social/twitter.png" alt="twitter">
        </a>
        <a @click="copiaCola">
          <img src="../assets/images/social/layer.png" alt="copy-paste">
        </a>
      </div>
    </div>
  </div>
</template>

<script>
import gsap from 'gsap';
import axios from 'axios';

export default {
  data() {
    return {
      isShuffling: false,
      startedWidget: false,
      images: [],
      selectedImage: null,
      stickerText: null,
      stickerShareText: null,
      currentIndex: 0,
      interval: null,
      showShareOptions: false,
      texto: '',
      url: ''
    }
  },
  created() {
    setTimeout(() => {
      axios.get('figurinhas.json')
        .then(res => {
          this.images = res.data;
        })
        .catch(error => {
          console.error("Erro ao carregar o JSON:", error);
        });
    }, 1000)
  },
  mounted() {
    const tl = gsap.timeline();
    const bala = document.querySelector('#bala');

    gsap.to('.heading-in', {
      opacity: 1,
      duration: .5
    })
    bala.addEventListener('click', () => {

      gsap.to('.heading-in', {
        opacity: 0,
        duration: .5
      })



      setTimeout(() => {
        this.startedWidget = true,
          this.startShuffling()

        setTimeout(() => {
          gsap.to('.heading-in', {
            opacity: 1,
            duration: .5
          })
          gsap.to('.white-container', {
            paddingBottom: '25px'
          })
          tl.to('.wrapper-figurinhas', {
            maxHeight: '360px',
            duration: .5,
            ease: 'power1.inOut'
          })
          tl.to('.wrapper-figurinhas', {
            duration: .5,
            opacity: 1,
            ease: 'power1.inOut'
          })
        }, 200)

      }, 300)
    })

    gsap.to('.fig-title', { opacity: 0 })

  },

  methods: {
    closeShare() {
      this.showShareOptions = false;
      console.log('share Closed!')
    },



    shareSticker() {
  this.showShareOptions = true;
  this.texto = `Eu tirei ${this.stickerText || "uma figurinha surpresa"}, qual você vai tirar?`;

  // Pede ao Webflow (pai) a URL atual
  window.parent.postMessage({ type: 'getParentUrl' }, '*');

  // Escuta a resposta do Webflow
  window.addEventListener('message', (event) => {
    if (event.data.type === 'parentUrl') {
      this.url = event.data.url;

      // Atualiza os botões e o texto de compartilhamento com a URL do pai
      console.log('URL recebida do Webflow:', this.url);
    }
  });
},
//shareSticker() {
//      this.showShareOptions = true;
//      this.texto = `Eu tirei ${this.stickerText || "uma figurinha surpresa"}, qual você vai tirar?`;
//      this.url = window.location.href

//    },

    startShuffling() {
      this.isShuffling = true;
      if (this.images.length === 0) return;

      this.isShuffling = true;
      this.interval = setInterval(() => {
        if (this.isShuffling) {
          this.currentIndex = (this.currentIndex + 1) % this.images.length;
          this.selectedImage = this.images[this.currentIndex].image;
          this.stickerText = this.images[this.currentIndex].title;
          this.stickerShareText = this.images[this.currentIndex].shareCopy;
        }
      }, 100);
    },



   // copiaCola() {
   //   const texto = `Eu tirei ${this.stickerText || "uma figurinha surpresa"}, qual você vai tirar?`;
   //   const url = window.location.href;
//
   //   const tempInput = document.createElement("input");
   //   tempInput.value = `${texto} ${url}`;
   //   document.body.appendChild(tempInput);
   //   tempInput.select();
   //   document.execCommand("copy");
   //   document.body.removeChild(tempInput);
   //   alert("Link copiado para a área de transferência!");
//
   //   this.closeShare()
   // },
    copiaCola() {
  const texto = `Eu tirei ${this.stickerText || "uma figurinha surpresa"}, qual você vai tirar?`;

  // solicita ao site pai (Webflow) a URL atual
  window.parent.postMessage({ type: "getParentUrl" }, "*");

  // escuta a resposta
  const listener = (event) => {
    if (event.data.type === "parentUrl") {
      const parentUrl = event.data.url;
      const tempInput = document.createElement("input");
      tempInput.value = `${texto} ${parentUrl}`;
      document.body.appendChild(tempInput);
      tempInput.select();
      document.execCommand("copy");
      document.body.removeChild(tempInput);
      alert("Link copiado para a área de transferência!");

      window.removeEventListener("message", listener);
      this.closeShare();
    }
  };

  window.addEventListener("message", listener);
},




    stopShuffling() {
      this.isShuffling = false;
      clearInterval(this.interval);
      setTimeout(() => {
        gsap.to('.heading-in', {
          opacity: 1,
          duration: .5
        })
      }, 100)
    }
  }
}

</script>

<style lang="scss">
h6 {
  line-height: 1.2em;
}

.white-container {
  min-width: 310px;
  height: auto;
  margin-bottom: 15px;
  gap: 10px;
  //transition: .5s;
  margin-top: 30px;
  justify-content: center;
  position: relative;
  background-color: white;
  padding: 10px;
  display: flex;
  align-items: start;
  border-radius: 20px;
  gap: 10px;
  margin-left: auto;
  margin-right: auto;
  transition: .5s;

  .heading-in {
    position: relative;
    background-color: white;
    padding: 10px;
    display: flex;
    align-items: start;
    border-radius: 20px;
    gap: 10px;
    margin-left: auto;
    margin-right: auto;
    opacity: 0;

    span {
      color: #7E1416
    }

    #hand {
      max-width: 24px;
      object-fit: contain;
    }

    h6 {
      font-size: 20px !important;
      max-width: 240px;
      text-align: left;
      margin: 0;
    }
  }

  .sorteio-imagens {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    .wrapper-figurinhas {
      opacity: 0;
      aspect-ratio: 9/16;
      overflow: hidden;
      max-height: 0;

      .imgs-figurinhas {
        aspect-ratio: 9/16;
        max-width: 200px;
        min-width: 200px;
        height: auto;
        object-fit: cover;
      }
    }


    .share-text {
      font-weight: 600;
      font-size: 12px;
      margin-top: 10px;
      text-transform: uppercase;
      max-width: 250px;
    }
  }
}




#lata-e-bala {
  position: relative;

  #lata {
    height: 80vh;
  }

  #bala {
    height: 52vh;
    position: absolute;
    bottom: 13px;
    left: 50%;
    transform: translatex(-50%);
    cursor: pointer;
    transition: filter .5s;

    &:hover {
      -webkit-filter: drop-shadow(1px 1px 1px white);
      filter: drop-shadow(1px 1px 1px white);
    }
  }

}

.btn {
  background-color: #7E1416;
  cursor: pointer;
  padding: 5px 10px;
  display: flex;
  flex-direction: row;
  border-radius: 50px;
  gap: 10px;
  margin-top: 20px;
  display: inline-flex;

  img {
    max-width: 13px !important;
    object-fit: contain;
  }

  span {
    font-weight: 700;
    color: white;
  }
}





.locked-background {
  position: fixed;
  top: 0;
  right: 0;
  width: 100vw;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;

  .share-options {
    background: white;
    padding: 40px;
    max-width: 320px;
    display: flex;
    gap: 10px;
    border-radius: 20px;
    height: fit-content;
    flex-direction: column;

    .share-buttons {
      display: flex;
      flex-direction: row;
      gap: 10px;
      justify-content: space-between;

    }

    img {
      max-width: 30px;
    }
  }
}

.centralize {
  h6 {
    text-align: center !important;
  }
}
</style>