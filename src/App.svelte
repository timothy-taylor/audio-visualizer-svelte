<script>
  import { interpolateInferno, interpolateSinebow } from "d3";
  import { ctx, startFromFile } from "./lib/audioSourceStore.js";
  import RadialGraph from "./lib/RadialGraph.svelte";

  let initState = true;
  const handlePlay = () => {
    if ($ctx.state === "suspended") $ctx.resume();
    startFromFile();
    initState = false;
  }
</script>

<main>
    {#if initState}
        <button on:click={handlePlay}>
            play
        </button>
    {:else}
        <svg
            width="100%"
            height="100%"
            viewBox="-100 -150 200 200"
            preserveAspectRatio="xMidYMid meet"
        >
            <RadialGraph color={interpolateSinebow} scale={2.0} />
            <RadialGraph color={interpolateInferno} scale={1.5} />
        </svg>
    {/if}
</main>

<style>
    main {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 100vh;
        width: 100vw;
    }

    button {
        font-family: Inter, sans-serif;
        font-size: 3rem;
        font-weight: bold;
        background-color: black;
        border: none;
        color: white;
        cursor: pointer;
    }
</style>