<script>
  import Footer from "../Footer.svelte";
  import Header from "../Header.svelte";
  import { onMount } from 'svelte';

  onMount(() => {
    // Esta funcion se ejecuta cuando la pagina se ha montado
    const html = document.querySelector('html')
    const header = document.querySelector('.header')
    const footer = document.querySelector('.footer')

    function setCSSVariables(){
      console.log('hola', html.offsetHeight)
      document.documentElement.style.setProperty('--html-height', `${html.offsetHeight}px`);
      document.documentElement.style.setProperty('--header-height', `${header.offsetHeight}px`);
      document.documentElement.style.setProperty('--footer-height', `${footer.offsetHeight}px`);
    }

    setCSSVariables()

    new ResizeObserver(setCSSVariables).observe(html);
    new ResizeObserver(setCSSVariables).observe(header);
    new ResizeObserver(setCSSVariables).observe(footer);
  });



</script>

<div id="app">
  <div class="header">
    <Header />
  </div>
  <div class="content">
    <slot />
  </div>
  <div class="footer">
    <Footer />
  </div>
</div>

<style>
  .content{
    display: flex;
    align-items: stretch;
    min-height: calc(var(--html-height, 100hv) - var(--header-height, 0px) - var(--footer-height, 0px));
  }

</style>
