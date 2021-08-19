<script>
    export let node;

    $: attributes = (node && node.attributes) || [];
</script>

{#each attributes as attr}{" "}<span class="text_attr_name">{attr.name}</span
    >{#if attr.value}{'="'}<span class="text_attr_value">{attr.value}</span
        >{'"'}{/if}{/each}

<style>
    .text_attr_name {
        color: #97bad7;
    }
    .text_attr_value {
        color: #f1946c;
    }
</style>
