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
  //


  //nuevo código

    let inputValue = "";
    let responseData = null;
    let urlServer = "http://localhost:3000";

    const handleSubmit = async () => {
      try {
        const response = await fetch(`${urlServer}/search?query=${inputValue}`);
        const data = await response.json();
        responseData = data;
      } catch (error) {
        console.error("Error al obtener los datos:", error);
      }
    };

  </script>

<div class="container">
    <main class="form-signin w-100 m-auto">
      <form>
        <img class="mb-4" src="/logo_corte.png" alt="" width="100" height="100">
        <div class="title">
          <h1 class="h3 mb-3 fw-normal">Buscador de relatoría</h1>
        </div>
        <div class="form-group">
          <input
            type="text"
            class="form-control form-control-lg"
            placeholder="Buscar..."
            bind:value={inputValue}
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
        <button class="w-100 btn btn-lg btn-primary" type="button" on:click={handleSubmit}>Buscar</button>
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
              <p><strong>Resumen</strong>
              <br>
              {item.summary}
              </p>
              <br>
              <p>{item.description}</p>
            </li>
          {/each}
        </ul>
      </div>
    </div>
    {#if responseData}
    <h2>Resultados</h2>
    <table>
      <thead>
        <tr>
          <th>Providencia</th>
          <th>Fecha sentencia</th>
          <th>Tipo</th>
          <th>Tipo proceso</th>
          <th>Demandante</th>
          <th>Resumen</th>
        </tr>
      </thead>
      <tbody>
        {#each responseData as row}
          <tr>
            <td>{row.metadata.Providencia}</td>
            <td>{row.metadata['Fecha Sentencia']}</td>
            <td>{row.metadata.Tipo}</td>
            <td>{row.metadata['Tipo de proceso']}</td>
            <td>{row.metadata.demandante}</td>
            <td>
              <textarea name="" id="" cols="40" rows="5">
                {row.metadata.summary}
              </textarea>
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  {/if}

  <style>
    table {
      border-collapse: collapse;
      width: 100%;
    }
    th,
    td {
      border: 1px solid black;
      padding: 8px;
      text-align: left;
    }
  </style>
  </div>
