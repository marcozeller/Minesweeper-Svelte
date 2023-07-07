<script>
    export let content;
    let state = "hidden";
    const clicked_on = () => {
        state = "open";
    };
</script>

<div class="cell">
    <button on:click={clicked_on}>
        {#if state == "hidden"}
            unknown
        {:else if state == "flagged"}
            flag
        {:else}
            {content}
        {/if}
    </button>
</div>

<style>
    .cell {
        height: 80px;
        width: 80px;
        background-image: url("covered.png");
    }
    .button {
        height: 80px;
        width: 80px;
    }
</style>
