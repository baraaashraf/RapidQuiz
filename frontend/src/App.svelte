<script lang="ts">
  import svelteLogo from "./assets/svelte.svg";
  import viteLogo from "/vite.svg";
  import Counter from "./lib/Counter.svelte";
  import Button from "./lib/button.svelte";
  import QuizCard from "./lib/QuizCard.svelte";
  let quizzes: { _id: string; name: string }[] = [];

  async function getQuizzes() {
    let response = await fetch("http://localhost:3000/api/quizzes");

    if (!response.ok) {
      alert("Failed!!");
      return;
    }

    let json = await response.json();
    quizzes = json;
    console.log(json);
  }

  function connect() {
    let websocket = new WebSocket("ws://localhost:3000/ws");
    websocket.onopen = () => {
      console.log("Opened connection");
      websocket.send("hello websocket");
    };

    websocket.onmessage = (event) => {
      console.log(event.data);
    };
  }
</script>

<button on:click={getQuizzes}>Get Quizzes</button>
<button on:click={connect}>Connect</button>

<Button text="Hii" />

<main>
  {#each quizzes as quiz}
  <QuizCard quiz={quiz}/>
  {/each}
</main>

<style>
  
</style>
