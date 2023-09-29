<script>
  import { onMount } from 'svelte';
  
  var resultados = []
  var conteos = []

  var requestSearch = {
    text: '',
    tipo: '',
    anio: '',
    fecha_inicio:'',
    fecha_fin: ''
  }

  onMount(async () => {
    var response = await fetch('http://localhost:5000/resultados')
    resultados = await response.json()
	});

  async function search(){
    var response = await fetch('http://localhost:5000/search', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(requestSearch)
    })
    resultados = await response.json()
  }

  async function count(){
    conteos = []
    if(requestSearch.anio == '') return alert('Selecciona año')
    var response = await fetch('http://localhost:5000/count', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(requestSearch)
    })
    conteos = await response.json()
  }
//
  let year = new Date().getFullYear();
  import Footer from './Footer.svelte';
  import Header from './Header.svelte';
  import { currentPage } from './store.js';
  import Estadisticas from './Estadisticas.svelte';
  import Info from './Info.svelte';
  import Contacto from './Contacto.svelte'
  import Chatbox from './Chatbox.svelte'
//
</script>

<div>
  <Header />

  {#if $currentPage === 'home'}
  <!-- home -->
{:else if $currentPage === 'informacion-corte'}
  <Info />
{:else if $currentPage === 'buscador-relatoria'}
<div class="container">
  <main class="form-signin w-100 m-auto">
    <form>
      <img class="mb-4" src="/logo_corte.png" alt="" width="72" height="57">
      <div class="title">
        <h1 class="h3 mb-3 fw-normal">Buscador de relatoría</h1>
      </div>
      <div class="form-group">
        <input
          type="text"
          class="form-control form-control-lg"
          placeholder="Buscar..."
          bind:value='{requestSearch.text}'
        />
      </div>
      <div class="flex-container">
      <div class="form-group">
        <label for="tipo">Tipo:</label>
        <select class="form-control" id="tipo" bind:value="{requestSearch.tipo}">
          <option value="Constitucional">Constitucional</option> 
          <option value="Auto">Auto</option>
          <option value="Tutela">Tutela</option>
        </select>
      </div>
      <div class="form-group">
        <label for="anio">Año:</label>
        <select class="form-control" id="anio" bind:value="{requestSearch.anio}">
          <option value="2023">2023</option>
          <option value="2022">2022</option>
          <option value="2021">2021</option>
          <option value="2020">2020</option>
          <option value="2019">2019</option>
          <option value="2018">2018</option>
        </select>
      </div>
      <div class="form-group">
        <label for="fecha_inicio">Fecha inicio:</label>
        <input type="date" class="form-control" id="fecha_inicio" bind:value="{requestSearch.fecha_inicio}"/>
      </div>
      <div class="form-group">
        <label for="fecha_fin">Fecha fin:</label>
        <input type="date" class="form-control" id="fecha_fin" bind:value="{requestSearch.fecha_fin}"/>
      </div>
    </div>
      <div class="checkbox mb-3">
      </div>
      <button class="w-100 btn btn-lg btn-primary" type="button" on:click={search}>Buscar</button>
      <div class="count">
      <button class="w-100 btn btn-lg btn-primary" type="button" on:click={count}>Contar</button>
      </div>
      <p class="mt-5 mb-3 text-body-secondary">&copy; 2017–2023</p>
    </form>
  </main>
  {#if conteos.length > 0}
  <div class="row justify-content-center">
    <div class="col-md-6">
    </div>
    <div class="col-md-6" id="form-container">
      <ul>
        {#each conteos as item}
          <li>
            <strong>{item.tipo_providencia}</strong>
            <br>
            <p>{item.count}</p>
          </li>
        {/each}
      </ul>
    </div>
  </div>
  {/if}
  <div class="row justify-content-center">
    <div class="col-md-6">
    </div>
    <div class="col-md-6" id="form-container">
      <ul>
        {#each resultados as item}
          <li>
            <strong>{item.title}</strong>
            <br>
            <p>{item.description}</p>
          </li>
        {/each}
      </ul>
    </div>
  </div>
</div>
  <!-- Componente o contenido del buscador de relatoría -->
{:else if $currentPage === 'estadisticas'}
  <Estadisticas />
{:else if $currentPage === 'contacto'}
  <Contacto />
{:else if $currentPage === 'chatbox'}
<Chatbox />
{/if}
</div>

<div>
  <Footer />
</div>
