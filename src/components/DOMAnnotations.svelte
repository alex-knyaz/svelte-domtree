<script>
  export let node;
  export let style = "background: #00ff9d5b;";

  $: top = node && offset(node).top;
  $: left = node && offset(node).left;
  $: width = node && node.offsetWidth;
  $: height = node && node.offsetHeight;


  // [ ] firefox table bug
  // https://stackoverflow.com/questions/1044988/getting-offsettop-of-element-in-a-table
  function offset(elem) {
    if (!elem) elem = this;

    var x = elem.offsetLeft;
    var y = elem.offsetTop;

    while ((elem = elem.offsetParent)) {
      x += elem.offsetLeft;
      y += elem.offsetTop;
    }

    return { left: x, top: y };
  }

  // [ ] svg?

</script>

<style>
  .annotation {
    position: absolute;
    pointer-events: none;
  }
</style>

{#if node}
  <div
    class="annotation"
    style="top:{top}px; left:{left}px; width:{width}px; height:{height}px; {style}" />
{/if}
