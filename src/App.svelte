<script lang="ts">
  import svelteLogo from "./assets/svelte.svg";
  import Counter from "./lib/Counter.svelte";
  import { createPopperActions } from "svelte-popperjs";

  const [ref, content, getInstance] = createPopperActions();

  const mousemove = (ev: MouseEvent) => {
    x = ev.clientX;
    y = ev.clientY;
  };
  let x = 0;
  let y = 0;
  $: getBoundingClientRect = () =>
    ({
      width: 0,
      height: 0,
      top: y,
      right: x,
      bottom: y,
      left: x,
    } as DOMRect);
  let virtualElement = { getBoundingClientRect };

  // reassign getBoundingClientRect whenever it updates and update the instance.
  // notice the comma (,) not semicolon (;) after the assignment.
  $: (virtualElement.getBoundingClientRect = getBoundingClientRect),
    getInstance()?.update();

  ref(virtualElement);
</script>

<svelte:window on:mousemove={mousemove} />

<main>
  <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite Logo" />
    </a>
    <img src={svelteLogo} class="logo svelte" alt="Svelte Logo" use:content />
  </div>

  <h1>Vite + Svelte</h1>

  <div class="card">
    <Counter />
  </div>

  <p>
    Check out <a href="https://github.com/sveltejs/kit#readme" target="_blank"
      >SvelteKit</a
    >, the official Svelte app framework powered by Vite!
  </p>

  <p class="read-the-docs">Click on the Vite and Svelte logos to learn more</p>
</main>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte {
    pointer-events: none;
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .read-the-docs {
    color: #888;
  }
</style>
