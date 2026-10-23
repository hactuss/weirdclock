<script lang="ts">
    // made by a human :>

    /* ToDo:
-[x] Visual division of porgress bar via gradient. (md on phone)
-[] show zero for n<10
-[x] grey/white upper limit for 1 hour bar that caps at 4
-[-] make visual division togglable
*/

    import { onMount } from "svelte";
    import { SvelteDate } from "svelte/reactivity";
    let date = new SvelteDate();
    let hour = $state(0);
    let minute = $state(0);
    let second = $state(0);

    let hour_sg1 = $state(0);
    let hour_sg2 = $state(0);
    let minute_sg1 = $state(0);
    let minute_sg2 = $state(0);
    let second_sg1 = $state(0);
    let second_sg2 = $state(0);

    let hour_max_single_1 = 2;
    let hour_max_single_2 = 9;
    let minute_max_single_1 = 5;
    let minute_max_single_2 = 9;
    let second_max_single_1 = 5;
    let second_max_single_2 = 9;
    /*
    if (new Date().getHours() >= 20) {
        hour_max_single_2 = 4;
        }*/

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

    let progress_hour_sg1 = $derived((100 / hour_max_single_1) * hour_sg1);
    let progress_hour_sg2 = $derived((100 / hour_max_single_2) * hour_sg2);
    let progress_minute_sg1 = $derived(
        (100 / minute_max_single_1) * minute_sg1,
    );
    let progress_minute_sg2 = $derived(
        (100 / minute_max_single_2) * minute_sg2,
    );
    let progress_second_sg1 = $derived(
        (100 / second_max_single_1) * second_sg1,
    );
    let progress_second_sg2 = $derived(
        (100 / second_max_single_2) * second_sg2,
    );

    let hour_default_width = 3;
    let minute_default_width = 2;
    let second_default_witdh = 1;

    let hour_width = $derived(hour_default_width);
    let minute_width = $derived(minute_default_width);
    let second_width = $derived(second_default_witdh);
    let show_time_per_segment = false;
    let segment_division_value = $state(0);
    let progress_cap_at_20 = $state(0);
    if (new Date().getHours() >= 20) {
        progress_cap_at_20 = (100 / hour_max_single_2) * 6;
    }
    function update_time() {
        const now = new Date();
        hour = now.getHours();
        minute = now.getMinutes();
        second = now.getSeconds();

        if (hour / 10 < 1) {
            hour_sg1 = 0;
            hour_sg2 = Array.from(String(hour), Number)[0];
        } else {
            hour_sg1 = Array.from(String(hour), Number)[0];
            hour_sg2 = Array.from(String(hour), Number)[1];
        }
        if (minute / 10 < 1) {
            minute_sg1 = 0;
            minute_sg2 = Array.from(String(minute), Number)[0];
        } else {
            minute_sg1 = Array.from(String(minute), Number)[0];
            minute_sg2 = Array.from(String(minute), Number)[1];
        }
        if (second / 10 < 1) {
            second_sg1 = 0;
            second_sg2 = Array.from(String(second), Number)[0];
        } else {
            second_sg1 = Array.from(String(second), Number)[0];
            second_sg2 = Array.from(String(second), Number)[1];
        }
    }
    onMount(() => {
        update_time();
        const interval = setInterval(update_time, 1000);
        return () => clearInterval(interval);
    });

    function defaultSegmentWidth() {
        hour_width = hour_default_width;
        minute_width = minute_default_width;
        second_width = second_default_witdh;
    }
</script>

{#snippet combined_segments()}
    <div id="segment_container">
        {@render segment(progress_hour_combined, 4)}
        {@render segment(progress_minute_combined, 2)}
        {@render segment(progress_second_combined, 1)}
    </div>
{/snippet}

{#snippet segment(
    progress: number,
    width: number,
    time_number: number,
    seperator_value: number,
    progress_cap: number,
)}
    <div>
        <div id="segment" style="width: {width}em;" style:--n={seperator_value}>
            <div id="segment_progress" style="height:{progress}%;"></div>
            <div
                id="segment_progress_cap"
                style="height: {progress_cap}%;"
            ></div>
        </div>
        {#if show_time_per_segment}
            <p style="color: orange">{time_number}</p>
        {/if}
    </div>
{/snippet}

{#snippet singular_segments()}
    <div id="segment_container">
        {@render segment(
            progress_hour_sg1,
            hour_width,
            hour_sg1,
            hour_max_single_1,
        )}
        {@render segment(
            progress_hour_sg2,
            hour_width,
            hour_sg2,
            hour_max_single_2,
            progress_cap_at_20,
        )}
        {@render segment(
            progress_minute_sg1,
            minute_width,
            minute_sg1,
            minute_max_single_1,
        )}
        {@render segment(
            progress_minute_sg2,
            minute_width,
            minute_sg2,
            minute_max_single_2,
        )}
        {@render segment(
            progress_second_sg1,
            second_width,
            second_sg1,
            second_max_single_1,
        )}
        {@render segment(
            progress_second_sg2,
            second_width,
            second_sg2,
            second_max_single_2,
        )}
    </div>
{/snippet}
<main>
    <p>{hour}:{minute}:{second}</p>
    {@render singular_segments()}
</main>
<br />
<footer>
    <a href="https://github.com/hactuss/weirdclock" id="github_repo_link"
        >Github repo</a
    >
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
        justify-content: space-between;
    }
    #segment_progress {
        background-color: slateblue;
        width: 100%;
    }
    #segment_progress_cap {
        background-color: grey;
        width: 100%;
    }
    #github_repo_link {
        position: absolute;
        bottom: 1em;
        right: 1em;
        &:hover {
            color: orange;
        }
        &:active {
            color: white;
        }
    }
    #segment {
        --n: 9;
        --line: 1px;
        --color: white;
        --bg: orange;
        background:
            linear-gradient(var(--bg) 0 0) top / 100% var(--line) no-repeat,
            repeating-linear-gradient(
                to top,
                transparent 0,
                transparent calc(100% / var(--n) - var(--line)),
                var(--color) calc(100% / var(--n) - var(--line)),
                var(--color) calc(100% / var(--n))
            );
        background-color: orange;
    }
</style>
