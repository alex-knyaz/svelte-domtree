<script>
  import PanAndZoom from "./PanAndZoom.svelte";
  import DOMAnnotations from "./DOMAnnotations.svelte";

  let root;

  export let node = document.createTextNode("root");

  $: {
    root &&
      (root.firstChild
        ? root.replaceChild(node, root.firstChild)
        : root.appendChild(node));
  }

  export let hovered = null;
  export let selected = null;

  const mousemove = (e) => {
    if (e.target == hovered) return;
    hovered = e.target;
  };
</script>

<PanAndZoom>
  <div bind:this={root} class="root" on:mousemove={mousemove} />

  <!-- COLORS DEFINED IN globals.scss -->

  <DOMAnnotations node={hovered} />
  <!-- style="border: solid 2px #4f555c; outline-offset: 2px;" -->
  <DOMAnnotations
    node={selected}
    style="outline: solid 1px #0061ac; outline-offset: -1px;"
  />
</PanAndZoom>
<!-- <iframe bind:this={wrapper} src="about:blank"/> -->

<style>
  .root {
    background: #fff;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
  }
</style>
