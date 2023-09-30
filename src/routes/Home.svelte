<script>
    let temasDeInteres = [
        {
            titulo: "Digitalización de Documentos Judiciales",
            descripcion: "La Corte ha iniciado un proceso para digitalizar todos los documentos judiciales para un acceso más fácil y transparente."
        },
        {
            titulo: "Plataforma de Consulta de Sentencias",
            descripcion: "Nueva plataforma en línea que permite a los ciudadanos consultar sentencias y otros documentos relacionados."
        },
        {
            titulo: "Inteligencia Artificial en el Sistema Judicial",
            descripcion: "Explorando el uso de IA para mejorar la eficiencia en el procesamiento y revisión de casos."
        }
    ];
</script>

<div class="container-corte">
    <img src="/img_corte.jpg" alt="Descripción de la imagen 2" width="350" />

    <h1>Temas de Interés en Transformación Tecnológica</h1>

    {#each temasDeInteres as tema}
    <div class="tema">
        <h2>{tema.titulo}</h2>
        <p>{tema.descripcion}</p>
    </div>
    {/each}
</div>
