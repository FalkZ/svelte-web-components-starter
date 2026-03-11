<svelte:options
    customElement={{
        tag: "modal-example",
        props: {
            open: { type: "Boolean", reflect: true },
            title: { type: "String" },
        },
    }}
/>

<script lang="ts">
    import Slot from "./slot.svelte";

    let { open = false, title = "" } = $props();

    let dialog = $state() as HTMLDialogElement;

    interface CommandEvent extends Event {
        command: string;
        source: HTMLElement;
    }

    // Invoker API: lets external buttons control this component via
    // commandfor / command attributes (see index.html for usage)
    $effect(() => {
        const handleCommand = (event: Event) => {
            const command = (event as CommandEvent).command;

            if (command === "show-modal") open = true;
            else if (command === "close") open = false;
        };

        $host().addEventListener("command", handleCommand);
        return () => $host().removeEventListener("command", handleCommand);
    });

    $effect(() => {
        if (open) {
            dialog.showModal();
        } else {
            dialog.close();
        }
    });

    const closeDialog = () => {
        open = false;
    };
</script>

<dialog
    bind:this={dialog}
    onclick={(e) => {
        // outside click closes dialog
        if (e.target === e.currentTarget) closeDialog();
    }}
>
    <div class="body">
        <div class="header">
            <h2 class="title">{title}</h2>
            <button class="close" aria-label="Close" onclick={closeDialog}>
                <svg
                    width="16"
                    height="16"
                    viewBox="0 0 16 16"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                >
                    <line x1="4" y1="4" x2="12" y2="12" />
                    <line x1="12" y1="4" x2="4" y2="12" />
                </svg>
            </button>
        </div>

        <div class="content">
            <Slot />
        </div>

        <div class="actions">
            <Slot name="actions" />
        </div>
    </div>
</dialog>

<style>
    dialog {
        padding: 0;
        border: none;
        border-radius: 8px;
        box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
        width: 28rem;
        max-width: 90%;
    }
    dialog::backdrop {
        background: rgba(0, 0, 0, 0.4);
    }
    .body {
        font-family: system-ui, sans-serif;
        padding: 1.5rem;
    }
    .header {
        display: flex;
        align-items: center;
        justify-content: space-between;
        margin-bottom: 0.75rem;
    }
    .title {
        margin: 0;
        font-size: 1.25rem;
        line-height: 1;
    }
    .close {
        cursor: pointer;
        background: none;
        border: none;
        border-radius: 6px;
        padding: 0.35rem;
        display: flex;
        align-items: center;
        justify-content: center;
        color: #888;
        transition:
            background 0.15s,
            color 0.15s;
    }
    .close:focus-visible {
        outline: none;
    }
    .close:hover {
        background: #f0f0f0;
        color: #333;
    }
    .content {
        color: #444;
        line-height: 1.5;
    }
    .actions {
        display: flex;
        justify-content: flex-end;
        gap: 0.5rem;
        margin-top: 1.25rem;
    }
</style>
