<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let content;
  export let x;
  export let y;
  export let state = "hidden";

  const clicked_on = () => {
    state = "open";
    if (content == "mine") {
      dispatch("loose_game", {});
    }
    if (content == 0) {
      dispatch("zero_uncovered", { x: x, y: y });
    }
  };
  const onRightClick = () => {
    if (state == "flagged") {
      state = "hidden";
      return;
    } else if (state == "hidden") {
      state = "flagged";
      return;
    } 
  }
</script>

{#if state == "hidden"}
  <div class="cell hidden center" on:click={clicked_on} on:contextmenu|preventDefault={onRightClick} />
{:else if state == "flagged"}
  <div class="cell flagged center" on:click={clicked_on} on:contextmenu|preventDefault={onRightClick} >
    <p>?</p>
  </div>
{:else}
  <div class="cell open center">
    {#if content == "mine"}
      <p>boom!</p>
    {:else}
      <p>{content}</p>
    {/if}
  </div>
{/if}

<style>
  .cell {
    height: 80px;
    width: 80px;
    border: solid grey 1px;
    margin: 2px;
    box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
    position: relative;
    text-align: center;
    vertical-align: middle;
  }
  .flagged {
  }
  .hidden {
    background-color: lightgrey;
    background-size: cover;
  }
  .center p {
    margin: 0;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
</style>
