<script lang="ts">
    import { onMount } from "svelte";
    import { SvelteDate } from "svelte/reactivity";
    let date = new SvelteDate();
    let hour = $state(0);
    let minute = $state(0);
    let second = $state(0);

    function update_time() {
        const now = new Date();
        hour = now.getHours();
        minute = now.getMinutes();
        second = now.getSeconds();
    }
    update_time();
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

    let progress_hour_combined = $derived((100 / hour_max_combined) * hour);
    let progress_minute_combined = $derived(
        (100 / minute_max_combined) * minute,
    );
    let progress_second_combined = $derived(
        (100 / second_max_combined) * second,
    );

    onMount(() => {
        update_time();
        const interval = setInterval(update_time, 1000);
        return () => clearInterval(interval);
    });
</script>

{#snippet segment(name: string, progress: number, width: number)}
    <div>
        <div id="segment" style="width: {width}em;">
            <div id="segment_progress" style="height:{progress}%;"></div>
        </div>
    </div>
{/snippet}

<h1>A weird clock</h1>
<main>
    <div id="segment_container">
        {@render segment("hour", progress_hour_combined, 4)}
        {@render segment("minute", progress_minute_combined, 2)}
        {@render segment("second", progress_second_combined, 1)}
    </div>
    <p>{hour}:{minute}:{second}</p>
</main>
<br />
<footer>
    <a href="https://github.com/hactuss/weirdclock">Github repo</a>
</footer>

<style lang="scss">
    :global {
        * {
            margin: 0;
            padding: 0;
            border: 0;
        }
        html {
            background-color: darkslateblue;
        }
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
    }
    :root {
        font-family: monospace;
        font-size: 1.5rem;
        color: white;
        text-align: center;
    }
    a {
        color: mediumpurple;
    }
    #segment_container {
        display: flex;
    }
    #segment {
        width: 1em;
        height: 7em;
        background-color: orange;
        display: flex;
        flex-direction: column-reverse;
        margin: 0.25em;
    }
    #segment_progress {
        background-color: slateblue;
        width: 100%;
    }
</style>
