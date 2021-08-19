<script>
  import DOMTreeNode from "./DOMTreeNode.svelte";

  export let node = null;
  export let selected = null;
  export let hovered = null;

  let mouseLeave = (e) => {
    hovered = null;
  };
</script>

<div class="main_grid_tree" on:mouseleave|preventDefault={mouseLeave}>
  <DOMTreeNode bind:node bind:selected bind:hovered />
</div>

<style>
  @import "../../global.scss";

  .main_grid_tree {
    overflow-y: scroll;
    background: #242424;

    color: #8d8d8d;
    font-family: monospace;
    font-size: 8.5pt;
    user-select: none;

    padding-left: 5px;
  }
</style>
