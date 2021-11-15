<script lang="ts">
    import { fade, fly } from "svelte/transition";
    import { elasticOut, cubicOut } from "svelte/easing";

    const slides = [
        "images/image_1.jpeg",
        "images/image_2.jpg",
        "images/image_3.jpg",
        "images/image_4.jpg",
    ];
    let selectedIndex = 0;
    let selectedSlide = slides[selectedIndex];
    let direction = -1;

    let prev = () => {
        selectedIndex = (selectedIndex - 1) % slides.length;
        if (selectedIndex < 0) selectedIndex = slides.length - 1;
        selectedSlide = slides[selectedIndex];
        direction = 1;
    };

    let next = () => {
        selectedIndex = (selectedIndex + 1) % slides.length;
        selectedSlide = slides[selectedIndex];
        direction = -1;
    };

    let preload = () => {
        return new Promise(function (resolve) {
            slides.forEach((e) => {
                let img = new Image();
                img.onload = resolve;
                img.src = e;
            });
        });
    };

    function myslide(
        node: Element,
        { delay = 0, duration = 400, easing = cubicOut, out = -1 }
    ) {
        const style = getComputedStyle(node);
        const upleft = -100;

        return {
            delay,
            duration,
            easing,
            css: (t, u) =>
                `transform: translate(${u * upleft * direction * out}%)`,
        };
    }
</script>

<svelte:head>
    <title>Head</title>
</svelte:head>

<div class="slide">
    {#await preload() then _}
        {#each slides as slide, i}
            {#if i === selectedIndex}
                <img
                    class="slide"
                    src={slide}
                    alt=""
                    in:myslide={{ duration: 2000, out: 1 }}
                    out:myslide={{ duration: 2000, out: -1 }}
                />
            {/if}
        {/each}
    {/await}
</div>

<a class="prev" on:click={prev}>&#10094;</a>
<a class="next" on:click={next}>&#10095;</a>

<style>
    .slide,
    .buttoncontainer {
        position: absolute;
        width: 100%;
        height: 100%;
        margin: 0%;
        padding: 0%;
        z-index: -1;
        right: 0%;
    }
    /* Next & previous buttons */
    .prev,
    .next {
        cursor: pointer;
        position: absolute;
        top: 50%;
        width: auto;
        margin-top: -22px;
        padding: 16px;
        color: white;
        font-weight: bold;
        font-size: 18px;
        transition: 0.6s ease;
        border-radius: 0 3px 3px 0;
        user-select: none;
    }

    /* Position the "next button" to the right */
    .next {
        right: 0;
        border-radius: 3px 0 0 3px;
    }

    /* On hover, add a black background color with a little bit see-through */
    .prev:hover,
    .next:hover {
        background-color: rgba(0, 0, 0, 0.8);
    }
</style>
