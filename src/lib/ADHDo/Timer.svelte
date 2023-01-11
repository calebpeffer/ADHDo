<script lang="ts">
    import { onMount } from 'svelte';
  
    let timer: number | undefined | Timer;
    let seconds = 0;
  
    function startTimer() {
      timer = setInterval(() => {
        seconds += 1;
      }, 1000);
    }
  
    function stopTimer() {
      clearInterval(timer);
      timer = undefined;
    }
  
    function resetTimer() {
      stopTimer();
      seconds = 0;
    }
  
    onMount(resetTimer);
    let minutes:number = 0;
    let sec: number = 0;

    $: { minutes = Math.floor(seconds / 60);}
    $: { sec = seconds % 60;}
  </script>
  
  <style>
    .timer {
      @apply text-4xl font-bold;
    }
  </style>
  
  <div class="timer bg-gray-200 rounded-lg p-4 flex flex-col items-center">
    <div>{minutes}:{sec}</div>
    <div class="mt-4">
      <button class="py-2 px-4 rounded-full bg-blue-500 text-white" on:click={startTimer}>
        Start
      </button>
      <button class="py-2 px-4 rounded-full bg-blue-500 text-white ml-4" on:click={stopTimer}>
        Stop
      </button>
      <button class="py-2 px-4 rounded-full bg-blue-500 text-white ml-4" on:click={resetTimer}>
        Reset
      </button>
    </div>
  </div>
  