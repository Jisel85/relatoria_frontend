<script>
  import { env } from "$env/dynamic/public";

  var requestSearch = {
    text: "",
    tipo: "",
    anio: "",
    fecha_inicio: "",
    fecha_fin: "",
    top_k: 20,
  };

  //nuevo código
  let responseData = null;

  const handleSubmit = async () => {
    try {
      if (!requestSearch.text) {
        alert("Debes escribir un texto");
        return;
      }
      const response = await fetch(`${env.PUBLIC_BACKEND_URL}/search`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(requestSearch),
      });
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
      <img class="mb-4" src="/logo_corte.png" alt="" width="100" height="100" />
      <div class="title">
        <h1 class="h3 mb-3 fw-normal">Buscador de relatoría</h1>
      </div>
      <div class="form-group">
        <input
          type="text"
          class="form-control form-control-lg"
          placeholder="Buscar..."
          bind:value={requestSearch.text}
        />
      </div>
      <div class="flex-container">
        <div class="form-group">
          <label for="tipo">Tipo:</label>
          <select
            class="form-control"
            id="tipo"
            bind:value={requestSearch.tipo}
          >
            <option value=""></option>
            <option value="Constitucionalidad">Constitucionalidad</option>
            <option value="Auto">Auto</option>
            <option value="Tutela">Tutela</option>
          </select>
        </div>
        <div class="form-group">
          <label for="anio">Año:</label>
          <select
            class="form-control"
            id="anio"
            bind:value={requestSearch.anio}
          >
            <option value=""></option>
            {#each Array.from({ length: 2023 - 1991 }, (_, i) => 2023 - i) as year}
              <option value={year}>{year}</option>
            {/each}
          </select>
        </div>
        <div class="form-group">
          <label for="fecha_inicio">Fecha inicio:</label>
          <input
            type="date"
            class="form-control"
            id="fecha_inicio"
            bind:value={requestSearch.fecha_inicio}
          />
        </div>
        <div class="form-group">
          <label for="fecha_fin">Fecha fin:</label>
          <input
            type="date"
            class="form-control"
            id="fecha_fin"
            bind:value={requestSearch.fecha_fin}
          />
        </div>
        <div class="form-group">
          <label for="fecha_fin"># Resultados:</label>
          <input
            type="number"
            class="form-control"
            id="top_k"
            bind:value={requestSearch.top_k}
          />
        </div>
      </div>
      <div class="checkbox mb-3"></div>
      <button
        class="w-100 btn btn-lg btn-primary"
        type="button"
        on:click={handleSubmit}>Buscar</button
      >
      <p class="mt-5 mb-3 text-body-secondary">&copy; 2017–2023</p>
    </form>
  </main>

  {#if responseData}
    <h2>Resultados</h2>
    <div class="table-responsive mt-4">
      <table class="table table-striped mb-0" role="grid">
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
              <td>{row.metadata["Fecha Sentencia"]}</td>
              <td>{row.metadata.Tipo}</td>
              <td>{row.metadata["Tipo de proceso"]}</td>
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
    </div>
  {/if}

  <style>
    .table-responsive{
      margin-top: 0px!important;
    }
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
    h2 {
      font-size: 20px;
      margin-top: 40px;
      color: white;
      background-color: #95c5d1;
      padding: 10px 10px;
      font-weight: bold;
      margin-bottom: 0;
      border-bottom: 1px solid black;
      text-align: center;
      text-shadow: 1px 1px 2px black;
    }
  </style>
</div>
