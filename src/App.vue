<template>
  <div id="app">
    <a class="typewrite"
      data-type='[ "i love you", "i like you", "i knew you", "hi"]'
    ></a>
  </div>
</template>

<script>
export default {
  name: 'App',
  mounted() {
    class TxtType {
      constructor(el, toRotate, period) {
        this.toRotate = toRotate;
        this.el = el;
        this.loopNum = 0;
        this.period = parseInt(period, 10) || 2000;
        this.txt = '';
        this.tick();
        this.isDeleting = false;
      }

      tick() {
        const i = this.loopNum % this.toRotate.length;
        const fullTxt = this.toRotate[i];

        if (this.isDeleting) this.txt = fullTxt.substring(0, this.txt.length - 1);
        else this.txt = fullTxt.substring(0, this.txt.length + 1);

        this.el.innerHTML = `<span class="wrap">${this.txt}</span>`;

        const that = this;
        let delta = 200 - Math.random() * 100;

        if (this.isDeleting) delta /= 2;

        if (!this.isDeleting && this.txt === fullTxt) {
          delta = this.period;
          this.isDeleting = true;
        } else if (this.isDeleting && this.txt === '') {
          this.isDeleting = false;
          this.loopNum += 1;
          delta = 500;
        }

        setTimeout(() => that.tick(), delta);
      }
    }

    const elements = document.getElementsByClassName('typewrite');

    for (let i = 0; i < elements.length; i += 1) {
      const toRotate = elements[i].getAttribute('data-type');
      const period = elements[i].getAttribute('data-period');

      if (toRotate) {
        new TxtType(elements[i], JSON.parse(toRotate), period);
      }
    }
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Swanky+and+Moo+Moo&display=swap');

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.typewrite {
  font-family: 'Swanky and Moo Moo', cursive;
  font-size: 4em;
  font-weight: 800;
  text-align: center;
}

body {
  margin: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
</style>
