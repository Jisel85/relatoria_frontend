<script>
  import { currentPage } from './store.js';
</script>

<nav>
  <button on:click={() => $currentPage = 'home'}>Inicio</button>
  <button on:click={() => $currentPage = 'informacion-corte'}>Información de la Corte Constitucional</button>
  <button on:click={() => $currentPage = 'buscador-relatoria'}>Buscador de Relatoría Judicial</button>
  <button on:click={() => $currentPage = 'estadisticas'}>Estadísticas</button>
  <button on:click={() => $currentPage = 'chatbox'}>Chatbot jurídico</button>
  <button on:click={() => $currentPage = 'contacto'}>Contacto</button>
</nav>
