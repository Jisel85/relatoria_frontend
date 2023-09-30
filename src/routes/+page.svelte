<script>
  import Footer from "./Footer.svelte";
  import Header from "./Header.svelte";
  import { currentPage } from "./store.js";
  import Estadisticas from "./Estadisticas.svelte";
  import Info from "./Info.svelte";
  import Contacto from "./Contacto.svelte";
  import Chatbox from "./Chatbox.svelte";
  import Buscador from "./buscador.svelte";
  import Home from "./Home.svelte";
</script>

<html lang="en">
  <body>
    <div class="principal">
      <div>
        <Header />
      </div>
      
      <div>
          {#if $currentPage === "home"}
            <Home />
        {:else if $currentPage === "informacion-corte"}
          <Info />
        {:else if $currentPage === "buscador-relatoria"}
          <Buscador />
        {:else if $currentPage === "estadisticas"}
          <Estadisticas />
        {:else if $currentPage === "contacto"}
          <Contacto />
        {:else if $currentPage === "chatbox"}
          <Chatbox />
        {/if}
      </div>
      
      <div>
        <Footer />
      </div>
    </div>
  </body>
</html>




