<script lang="ts">
  export let date: Date;
</script>

<div>
  {#if date.getDate() === 1}
    {date.getMonth() + 1}/{date.getDate()}
  {:else}
    {date.getDate()}
  {/if}
</div>

<style>
  div {
    flex: 1;
    border: 1px solid #ccc;
    border-top-width: 0;
  }

  div:nth-child(n + 2) {
    border-left-width: 0;
  }
</style>
