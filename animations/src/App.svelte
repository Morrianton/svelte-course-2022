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
    let showParagraph = false;
    let showAdded = false;
    let showDiscarded = false;

    function addBox() {
        boxes = [ ...boxes, boxInput.value ]
        boxInput.value = '';
        showAdded = true;
        setTimeout(() => {
            showAdded = false;
        }, 500);
    }

    function discard(value) {
        boxes = boxes.filter(box => box !== value);
        showDiscarded = true;
        setTimeout(() => {
            showDiscarded = false;
        }, 500);
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

<button
    on:click="{() => showParagraph = !showParagraph}">
    Toggle
</button>

{#if showParagraph}
    <p
        transition:fly="{
            { x: 300 }
        }">
        Can you see me?
    </p> 
{/if}

<hr>

<input type="text" bind:this="{boxInput}">
<button on:click="{addBox}">Add</button>
{#if showAdded}
    <span
        out:fade="{
            { delay: 250 }
        }">
        Added!
    </span>
{/if}

{#if showDiscarded}
    <span
        out:fade="{
            { delay: 250 }
        }">
        Discarded!
    </span>
{/if}

{#if showParagraph}
    {#each boxes as box (box)}
        <div
            on:click="{discard.bind(this, box)}"
            transition:fly|local="{
                {
                    easing: cubicIn,
                    x: -200,
                    y: 0
                }
            }"
            on:introstart="{() => console.log('Adding the element starts.')}"
            on:introend="{() => console.log('Adding the element end.')}"
            on:outrostart="{() => console.log('Discarding the element starts.')}"
            on:outroend="{() => console.log('Discarding the element ends.')}">
            {box}
        </div>
    {/each}
{/if}
