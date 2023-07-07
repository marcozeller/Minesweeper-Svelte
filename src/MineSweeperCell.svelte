<script>
    export let content;
    let state = "hidden";
    const clicked_on = () => {
        state = "open";
    };
</script>

{#if state == "hidden"}
    <div class="cell hidden" on:click={clicked_on} />
{:else if state == "flagged"}
    <div class="cell flagged" on:click={clicked_on}>?</div>
{:else}
    <div class="cell open">{content}</div>
{/if}

<style>
    .cell {
        height: 80px;
        width: 80px;
    }
    .flagged {
    }
    .hidden {
        background-image: url("http://i47.tinypic.com/2ni0ahd.png");
        background-size: cover;
    }
</style>
