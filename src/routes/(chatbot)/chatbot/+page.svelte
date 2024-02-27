<script>
  import { env } from "$env/dynamic/public";

  /**
   * @type {any[]}
   */
  let messages = [];
  let newMessage = "";
  let isLoading = false;

  function sendMessage() {
    if (newMessage.trim() !== "") {
      messages = [
        ...messages,
        {
          message: newMessage.trim(),
          isOwn: true,
        },
      ];
      askToBot(newMessage.trim());
      newMessage = "";
      setTimeout(scrollToEnd, 300);
    }
  }

  /**
   * @param {string} message
   */
  async function askToBot(message) {
    isLoading = true;
    try{
      const response = await fetch(
        `${env.PUBLIC_BACKEND_URL}/chatbot?query=${message}`,
      );
      const data = await response.json();
      messages = [
        ...messages,
        {
          message: data.response.replace(/#{2,}/g, '<br><br>'),
          isOwn: false,
        },
      ];
      setTimeout(scrollToEnd, 300);
    } catch(_){}
    isLoading = false
  }

  function scrollToEnd() {
    const div = document.querySelector(".messages");
    div.scrollTop = div?.scrollHeight;
  }
</script>

<div class="container">
  <div class="chatbox">
    <div class="image">
      <img
        src="/img_diosa_loros.png"
        alt="Descripción de la imagen 1"
        width="20%"
      />
    </div>
    <div class="messages">
      <ul class="list-group">
        {#each messages as message}
          {#if message.isOwn}
            <li class="list-group-item">{message.message}</li>
          {:else}
            <li class="list-group-item chatbox-response">{@html message.message}</li>
          {/if}
        {/each}
        {#if isLoading}
          <div class="loader list-group-item chatbox-response">Cargando</div>
        {/if}
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
  .messages {
    flex: 1;
    overflow-y: auto;
  }
  .list-group-item {
    text-align: right;
    margin-bottom: 12px;
  }

  .chatbox-response {
    background-color: #d5e4ff;
    text-align: left;
  }

  .loader {
    position: relative;
    height: 20px;
    display: block;
    box-sizing: content-box;
  }

  .loader::after {
    bottom: 110%;
    left: 10%;
    position: absolute;
    box-sizing: content-box;
    display: inline-block;
    height: 20px;
    font-size: 50px;
    content: " .";
    animation: dots 1.5s infinite;
  }

  @keyframes dots {
    0%,
    20% {
      color: transparent;
      text-shadow:
        0.25em 0 0 transparent,
        0.5em 0 0 transparent;
    }
    40% {
      color: blue;
      text-shadow:
        0.25em 0 0 blue,
        0.5em 0 0 transparent;
    }
    60% {
      text-shadow:
        0.25em 0 0 blue,
        0.5em 0 0 blue;
    }
    80%,
    100% {
      color: transparent;
      text-shadow:
        0.25em 0 0 transparent,
        0.5em 0 0 blue;
    }
  }
</style>
