<script>
    let datos = [
      { nombre: "Casos Procesados", valor: 1500 },
      { nombre: "Casos Pendientes", valor: 200 },
      { nombre: "Casos Exitosos", valor: 1350 },
      { nombre: "Casos Denegados", valor: 150 },
    ];
  
    // Estadísticas Descriptivas Ficticias
    let media = 1000;
    let mediana = 950;
    let moda = "Casos Exitosos";
    let desviacionEstandar = 150;
  </script>

<div class="estadisticas">
    <h1>Estadísticas Descriptivas</h1>
  
    {#each datos as dato (dato.nombre)}
      <div class="dato">
        <strong>{dato.nombre}:</strong> {dato.valor}
      </div>
    {/each}
  
    <h2>Resumen Estadístico</h2>
    <div class="dato">
      <strong>Media:</strong> {media}
    </div>
    <div class="dato">
      <strong>Mediana:</strong> {mediana}
    </div>
    <div class="dato">
      <strong>Moda:</strong> {moda}
    </div>
    <div class="dato">
      <strong>Desviación Estándar:</strong> {desviacionEstandar}
    </div>
  </div>