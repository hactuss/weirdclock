<script lang="ts">
    import { onMount } from "svelte";
    import { SvelteDate } from "svelte/reactivity";

    let date = new SvelteDate();
    let hour = $state(date.getHours());
    let minute = $state(date.getMinutes());
    let second = $state(date.getSeconds());

    onMount(() => {
        const interval = setInterval(() => {
            console.log(new SvelteDate());
        }, 1000);
        return () => clearInterval(interval);
    });

    let hour_max_single_1 = 2;
    let hour_max_single_2 = 3;
    let minute_max_single_1 = 5;
    let minute_max_single_2 = 9;
    let second_max_single_1 = 5;
    let second_max_single_2 = 9;

    let hour_max_combined = 23;
    //hour_max_single_1 + hour_max_single_2
    let minute_max_combined = 59;
    //minute_max_single_1 + minute_max_single_2
    let second_max_combined = 59;
    //second_max_single_1 + second_max_single_2

    let progress_hour_combined = (100 / hour_max_combined) * hour;
    let progress_minute_combined = (100 / minute_max_combined) * minute;
    let progress_second_combined = (100 / second_max_combined) * second;
</script>

{#snippet segment(name: string, progress: number)}
    <div>
        <div id="segment">
            <div id="segment_progress" style="height:{progress}%;"></div>
        </div>
        <p>{progress}%</p>
        <p>{name}</p>
    </div>
{/snippet}

<h1>A weired clock</h1>
<main>
    <div id="segment_container">
        {@render segment("hour", progress_hour_combined)}
        {@render segment("minute", progress_minute_combined)}
        {@render segment("second", progress_second_combined)}
    </div>
</main>

<style lang="scss">
    :global {
        * {
            margin: 0;
            padding: 0;
            border: 0;
        }
        html {
            background-color: lightslategrey;
        }
        body {
        }
    }
    :root {
        font-size: 2rem;
    }
    button {
        background-color: white;
        padding-inline: 3em;
        padding-block: 1em;
    }
    #segment_container {
        display: flex;
    }
    #segment {
        width: 2em;
        height: 4em;
        background-color: orange;
        display: flex;
        flex-direction: column-reverse;
    }
    #segment_progress {
        background-color: slateblue;
        width: 100%;
    }
</style>
