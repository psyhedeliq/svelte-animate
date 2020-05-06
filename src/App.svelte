<script>
  import { writable } from "svelte/store";
  import { tweened } from "svelte/motion";
  import { cubicIn } from "svelte/easing";
  import { fade, fly, slide, scale } from "svelte/transition";
  import Spring from "./Spring.svelte";

  let boxInput;
  let showParagraph = false;

  const progress = tweened(0, {
    delay: 0,
    duration: 700,
    easing: cubicIn
  });

  setTimeout(() => {
    progress.set(0.5);
  }, 1500);

  let boxes = [];

  const addBox = () => {
    boxes = [...boxes, boxInput.value];
  };

  const discard = value => {
    boxes = boxes.filter(el => el !== value);
  };
</script>

<style>
  div {
    width: 10rem;
    height: 10rem;
    background: #ccc;
    margin: 1rem;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
    border-radius: 6px;
    padding: 1rem;
  }
</style>

<!-- <progress value={$progress} /> -->
<!-- <Spring /> -->

<button
  on:click={() => {
    showParagraph = !showParagraph;
  }}>
  Toggle
</button>
{#if showParagraph}
  <p transition:fly={{ x: 300 }}>Can you see me?</p>
{/if}

<hr />

<input type="text" bind:this={boxInput} />
<button on:click={addBox}>Add</button>

{#if showParagraph}
  {#each boxes as box (box)}
    <!-- <div transition:fade>{box}</div> -->
    <!-- <div transition:scale>{box}</div> -->
    <!-- <div transition:slide>{box}</div> -->
    <!-- <div transition:fly={{ easing: cubicIn }}>{box}</div> -->
    <!-- <div transition:scale={{ easing: cubicIn, start: 0.5, opacity: 0.5 }}>
    {box}
  </div> -->
    <!-- <div transition:fly={{ easing: cubicIn, x: 0, y: 300 }}>{box}</div> -->
    <div
      transition:fly|local={{ x: 200, y: 0 }}
      on:click={discard.bind(this, box)}
      on:introstart={() => console.log('adding the element starts')}
      on:introend={() => console.log('adding the element end')}
      on:outrostart={() => console.log('remove the element starts')}
      on:outroend={() => console.log('remove the element end')}>
      {box}
    </div>
  {/each}
{/if}
