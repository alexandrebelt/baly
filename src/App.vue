<template>
  <HomePage/>
</template>

<script>
import HomePage from './components/HomePage.vue'

export default {
  name: 'App',
  components: {
    HomePage
  }
}
</script>

<style lang="scss">
@font-face {
  font-family: 'Mont';
  src: url('./assets/font/mont/Mont-Thin.otf');
  font-weight: 100;
}
@font-face {
  font-family: 'Mont';
  src: url('./assets/font/mont/Mont-ExtraLight.otf');
  font-weight: 200;
}
@font-face {
  font-family: 'Mont';
  src: url('./assets/font/mont/Mont-Light.otf');
  font-weight: 300;
}
@font-face {
  font-family: 'Mont';
  src: url('./assets/font/mont/Mont-Regular.otf');
  font-weight: 400;
}
@font-face {
  font-family: 'Mont';
  src: url('./assets/font/mont/Mont-SemiBold.otf');
  font-weight: 600;
}
@font-face {
  font-family: 'Mont';
  src: url('./assets/font/mont/Mont-Bold.otf');
  font-weight: 700;
}
@font-face {
  font-family: 'Mont';
  src: url('./assets/font/mont/Mont-Black.otf');
  font-weight: 800;
}

@font-face {
  font-family: 'Mont';
  src: url('./assets/font/mont/Mont-ThinItalic.otf');
  font-weight: 100;
  font-style: italic;
}
@font-face {
  font-family: 'Mont';
  src: url('./assets/font/mont/Mont-ExtraLightItalic.otf');
  font-weight: 200;
  font-style: italic;
}
@font-face {
  font-family: 'Mont';
  src: url('./assets/font/mont/Mont-LightItalic.otf');
  font-weight: 300;
  font-style: italic;
}
@font-face {
  font-family: 'Mont';
  src: url('./assets/font/mont/Mont-RegularItalic.otf');
  font-weight: 400;
  font-style: italic;
}
@font-face {
  font-family: 'Mont';
  src: url('./assets/font/mont/Mont-SemiBoldItalic.otf');
  font-weight: 600;
  font-style: italic;
}
@font-face {
  font-family: 'Mont';
  src: url('./assets/font/mont/Mont-BoldItalic.otf');
  font-weight: 700;
  font-style: italic;
}
@font-face {
  font-family: 'Mont';
  src: url('./assets/font/mont/Mont-BlackItalic.otf');
  font-weight: 800;
  font-style: italic;
}

*{padding: 0;
margin: 0;}
html,
body {
  overflow: hidden;
}

body{
  font-family:'Mont';
  background: radial-gradient(#D4223E, #7E1416);
  background-repeat: no-repeat;
  background-size: cover;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;

}
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  position: absolute;
  top: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  text-align: center;
}

</style>
