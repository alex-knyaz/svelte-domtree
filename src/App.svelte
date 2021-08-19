<script>
  import DOMShower from "./components/DOMShower.svelte";
  import DOMTree from "./components/DOMTree/DOMTree.svelte";
  import test_page from "./test_pages/test_html.html";

  let htmlDoc = new DOMParser().parseFromString(test_page, "text/html");
  let doc = htmlDoc.documentElement;

  let selected;
  let hovered;

  $: {
    if (selected && !selected.nodeType == Node.COMMENT_NODE)
      selected.scrollIntoView({ block: "nearest" });
  }
</script>

<div class="main_grid">
  <DOMShower node={doc} bind:hovered bind:selected />
  <DOMTree node={doc} bind:hovered bind:selected />

  <p class="discription">
    Midlle click: pan, <br /><kbd>ctrl</kbd> + Mouse weel: zoom <br />
    <a href="https://github.com/alex-knyaz/svelte-domtree">Project page</a>
  </p>
</div>

<style>
  .discription {
    position: absolute;
    padding: 10px;
    margin: 20px;
    border: solid gray 1px;
    background-color: #f9f9f9;
  }

  .main_grid {
    margin: 0px;
    height: calc(100vh);
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 3px 3px;
  }

  :global(body) {
    margin: 0;
    padding: 0;
  }
</style>
