<script>
  import { onMount } from 'svelte';

  let messages =  [];
  let newMessage = "";

  function sendMessage() {
    if (newMessage.trim() !== "") {
      messages = [...messages, newMessage];
      newMessage = "";
      setTimeout(scrollToEnd, 300);
    }
  }

  function scrollToEnd(){
    const div = document.querySelector('.messages')
    div.scrollTop = div?.scrollHeight
  }
</script>

<div class="container">
  <div class="chatbox">
    <div class="image">
      <img
        src="/chatbot_post.png"
        alt="Descripción de la imagen 1"
        width="20%"
      />
    </div>
    <div class="messages">
      <ul class="list-group">
        {#each messages as message}
          <li class="list-group-item">{message}</li>
        {/each}
      </ul>
    </div>
    <div>
      <div class="input-group">
        <input
          bind:value={newMessage}
          on:keyup={(e) => (e.key === "Enter" ? sendMessage() : "")}
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
</div>

<style>
  .chatbox {
    height: 100%;
    display: flex;
    flex-direction: column;
    padding-bottom: 20px;
  }
  .image {
    text-align: center;
    padding: 20px 0;
  }
  .messages{
    flex: 1;
    overflow-y: auto;
  }

</style>
