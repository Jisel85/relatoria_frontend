<script>
    export let messages = [];
  let newMessage = '';

  function sendMessage() {
    if (newMessage.trim() !== '') {
      messages = [...messages, newMessage];
      newMessage = '';
    }
  }
</script>

<div class="images">
    <img src="/chatbot_post.png" alt="Descripción de la imagen 1" width=20% />
</div>

<div class="card">
    <div class="card-body">
      <ul class="list-group">
        {#each messages as message (message)}
          <li class="list-group-item">{message}</li>
        {/each}
      </ul>
    </div>
    <div class="card-footer">
      <div class="input-group">
        <input
          bind:value={newMessage}
          on:keyup={(e) => (e.key === 'Enter' ? sendMessage() : '')}
          type="text"
          class="form-control"
          placeholder="Escribe tu mensaje..."
        />
        <div class="input-group-append">
          <button on:click={sendMessage} class="btn btn-primary">Enviar</button>
        </div>
      </div>
    </div>
  </div>