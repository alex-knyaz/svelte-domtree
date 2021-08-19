<!-- svelte-ignore a11y-mouse-events-have-key-events -->
<script>
  import DomAttrib from "./DOMAttrib.svelte";

  const offset = 11;

  export let node;
  export let selected = null;
  export let hovered = null;
  export let depth = 0;

  let isText = node && node.nodeType == Node.TEXT_NODE;

  let isComment = node && node.nodeType == Node.COMMENT_NODE;

  let isEmpty = node && node.childNodes.length == 0;

  let isEmptyText = isText && node.nodeValue.trim() == "";

  let hasOnlyChild =
    node &&
    node.childNodes.length == 1 &&
    node.firstChild.nodeType == Node.TEXT_NODE;

  let name = node && node.tagName && node.tagName.toLowerCase();

  $: isSelected = node == selected;
  $: isHovered = node == hovered;

  let isOpen = true;

  let select = (e) => {
    if (selected == node) return;
    selected = node;
  };

  let mouseEnter = (e) => {
    if (hovered == node) return;
    hovered = node;
  };
</script>

{#if isEmptyText}
  <!-- Nothing -->
{:else if isComment}
  <div
    style="padding-left:{offset * (depth + 1)}px;"
    class="text_comment"
    on:click={select}
    on:mouseover={mouseEnter}
    class:selected={isSelected}
    class:hovered={isHovered}
  >
    {"<!--" + node.nodeValue + "-->"}
  </div>
{:else if isText}
  <div
    class="text_text"
    style="padding-left:{offset * (depth + 1)}px;"
    on:click={select}
    on:mouseover={mouseEnter}
    class:selected={isSelected}
    class:hovered={isHovered}
  >
    {'"' + node.nodeValue + '"'}
  </div>
{:else if isEmpty}
  <div
    class="text_tag"
    style="padding-left:{offset * (depth + 1)}px;"
    on:click={select}
    on:mouseover={mouseEnter}
    class:selected={isSelected}
    class:hovered={isHovered}
  >
    {"<" + name}<DomAttrib {node} />{"></" + name + ">"}
  </div>
{:else if hasOnlyChild}
  <div
    class="text_tag"
    style="padding-left:{offset * (depth + 1)}px;"
    on:click={select}
    on:mouseover={mouseEnter}
    class:selected={isSelected}
    class:hovered={isHovered}
  >
    {"<" + name}<DomAttrib {node} />{">"}<span class="text_text"
      >{node.firstChild.nodeValue}</span
    >{"</" + name + ">"}
  </div>
{:else}
  <details
    bind:open={isOpen}
    on:click|self={select}
    class:selected={isSelected}
    class:hovered={isHovered}
    on:mouseover|self={mouseEnter}
  >
    <summary
      style="margin-left:{offset * depth}px; "
      class="summary"
      on:mouseover={mouseEnter}
      tabIndex="-1"
    >
      <span class="text_tag summary_span" on:click|preventDefault={select}>
        {"<" + name}<DomAttrib {node} />{">"}
        {#if !isOpen}
          <span class="text_text">…</span>
          {"</" + name + ">"}
        {/if}
      </span>
    </summary>
    {#if isOpen}
      {#each node.childNodes as child}
        <svelte:self
          node={child}
          depth={depth + 1}
          bind:selected
          bind:hovered
        />
      {/each}

      <span
        class="text_tag"
        on:click|self={select}
        on:mouseover|self={mouseEnter}
        style="padding-left:{offset * (depth + 1)}px;">{"</" + name + ">"}</span
      >
    {/if}
  </details>
{/if}

<style>
  .hovered {
    background: #4f555c;
  }

  .selected {
    background: #0061ac;
  }

  .text_tag {
    color: #56b1d3;
  }
  .text_text {
    color: #cfd0d0;
  }
  .text_comment {
    color: #86d362;
    white-space: pre-wrap;
  }
  .summary {
    outline: none;
  }
  .summary_span {
    /* hack - firefox */
    width: calc(100% - 13px);
    display: inline-block;
  }
</style>
