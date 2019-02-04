<template>
  <div id="app" class="container">
    <img alt="Branding" class="app-branding" src="./assets/logo.png">
    <p class="app-name">Base64 DataURI generator</p>
    <router-view/>
    <p class="app-footer">Made with <span class="red">♥</span> by <a href="https://tammai.me">Tam M.</a></p>
  </div>
</template>

<style lang="scss">
*, *:before, *:after {
  box-sizing: border-box;
}

html {
  font-size: 10px;
}

body {
  margin: 0;

  font-family: 'Avenir', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  font-size: 1.6rem;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.container {
  max-width: 64rem;
  margin: 0 auto;
  padding: 4rem 2.4rem;
}

.app-name {
  margin: 2.4rem 0 4rem;

  font-size: 1.2rem;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}

.app-branding {
  display: block;

  width: 8rem;
  height: 8rem;
  margin: 0 auto;

}

.app-footer {
  margin: 0;
  padding-top: 4rem;

  font-size: 1rem;
  text-align: center;
  text-transform: uppercase;
  letter-spacing: .5px;
  color: #999;

  a {
    color: #999;
    text-decoration: none;
  }
}

.red {
  color: #c0392b;
}
</style>
