<script>
    // import { writable } from "svelte/store";
    // import { tweened } from "svelte/motion";
    import { cubicIn } from "svelte/easing";
    import Spring from "./Spring.svelte";
    import { fade, fly, slide, scale } from "svelte/transition";

    // const progress = tweened(0, {
    //     delay: 0,
    //     duration: 700,
    //     easing: cubicIn
    // });

    // setTimeout(() => {
    //     progress.set(0.5);
    // }, 1500);

    let boxes = [];
    let boxInput;

    function addBox() {
        boxes = [ ...boxes, boxInput.value ]
    }

    function discard(value) {
        boxes = boxes.filter(box => box !== value);
    }
</script>

<style>
    div {
        width: 10rem;
        height: 10rem;
        background-color: #ccc;
        margin: 1rem;
        box-shadow:
            0 1px 1px hsl(0deg 0% 50% / 0.333),
            0 2px 2px hsl(0deg 0% 50% / 0.333),
            0 4px 4px hsl(0deg 0% 50% / 0.333),
            0 6px 6px hsl(0deg 0% 50% / 0.333);
        border-radius: 5px;
        padding: 1rem;
    }
</style>

<!-- <progress value="{$progress}"></progress> -->
<!-- <Spring></Spring> -->
<input type="text" bind:this="{boxInput}">
<button on:click="{addBox}">Add</button>

{#each boxes as box (box)}
    <div
        on:click="{discard.bind(this, box)}"
        transition:fly="{
            {
                easing: cubicIn,
                x: -200,
                y: 0
            }
        }">
        {box}
    </div>
{/each}
