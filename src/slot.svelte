<script lang="ts">
    import type { Snippet } from "svelte";

    type Props = {
        name?: string;
        children?: Snippet;
    };

    let { name, children }: Props = $props();
</script>

<svelte:element this={"slot"} {name}>
    {@render children?.()}
</svelte:element>
