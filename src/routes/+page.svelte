<!-- <script context="module"> -->
<!--   export const prerender = true; -->
<!-- </script> -->

<script lang="ts">
  import { onMount } from 'svelte';
  import { Application, Sprite, Assets } from 'pixi.js';

  let app;

  onMount(async () => {
    const app = new Application();
    await app.init({ background: '#1099bb', resizeTo: window });
    document.getElementById('content-div')?.appendChild(app.canvas);

    const texture = await Assets.load('https://pixijs.com/assets/bunny.png');

    const bunny = new Sprite(texture);
    app.stage.addChild(bunny);
    bunny.anchor.set(0.5);
    bunny.x = app.screen.width / 2;
    bunny.y = app.screen.height / 2;
    console.log('nanin');
  });
</script>

<h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>

<div id="content-div"></div>
