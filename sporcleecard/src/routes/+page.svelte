<script>
    import { onMount } from "svelte";
    import { cubicInOut, quintOut } from "svelte/easing";
    import { draw, fly, scale, slide } from "svelte/transition";

    let toggle = $state(false);

    let poem_rows = [
        { letter: 'S', words: 'Super fun' },
        { letter: 'P', words: 'Playing with friends' },
        { letter: 'O', words: 'Only words can express a fraction of my gratitude' },
        { letter: 'R', words: 'Racing the clock' },
        { letter: 'C', words: 'Cherish do I your wealth of knowledge' },
        { letter: 'L', words: 'Learning new facts' },
        { letter: 'E', words: 'Every game is better than the last' }
    ]

    let page = $state(0);
    let pagesEnd = 1;

    let buttonActive = $state(0);

    let visibleRows = $state(new Set());

    function clickLeft() {
        if (page == 2) {
            page--;
            visibleRows = new Set();
            poem_rows.forEach((_, i) => {
                setTimeout(() => {
                    visibleRows = new Set(visibleRows).add(i);
                }, i * 1500)
            }); 
        } else {
            page--;
        }
    };

    function clickRight() {
        if (page == 0) {
            page++;
            visibleRows = new Set();
            poem_rows.forEach((_, i) => {
                setTimeout(() => {
                    visibleRows = new Set(visibleRows).add(i);
                }, i * 1500)
            }); 
        } else {
            page++;
        }
    };

    onMount(() => {
        setTimeout(() => {toggle = true}, 100);
    });
</script>
<style>
    #card {
        position: fixed;
        background-color: #eddfd6;
        top: 50px;
        bottom: 50px;
        right: 120px;
        left: 120px;
        border-radius: 20px;
        box-shadow: 5px 10px rgb(197, 181, 181), 20px 20px 25px rgba(0, 0, 0, 0.39);
        animation: pulse 3s infinite ease-in-out;
        transition: box-shadow 0.3s ease-in-out;
    }

    #card:hover {
        box-shadow: 8px 13px rgb(167, 152, 152), 30px 30px 25px rgba(0, 0, 0, 0.39);
    }

    @keyframes pulse {
        0% {
            transform: scale(1);
        }
        50% {
            transform: rotate(0deg) scale(1.004);
        }
        100% {
            transform: scale(1);
        }
    }

    #content {
        position: relative;
        width: 100%;
        height: 100%;
    }

    #page {
        position: absolute;
        transform: translate(-50%, -50%);
        top: 50%;
        left: 50%;
    }

    #page h2 {
        margin-left: 30px;
        margin-right: 30px;
    }

    #buttons {
        position: absolute;
        bottom: 30px;
        transform: translate(-50%, 0%);
        left: 50%;
    }

    #table {
        width: 300px;
        height: 400px;
    }

    #table, tr, td {
        border: 1px solid black;
        border-collapse: collapse;
    }

    #table tr {
        height: calc(100% / 7);
    }

    #table td {
        text-align: center;
        padding: 10px;
    }

    #table td:first-child {
        width: 20%;
        background-color: #fe9900;
    }

    #table td:last-child {
        background-color: white;
    }
</style>

{#if toggle}
    <div id="card" transition:fly={{y:-200, easing: cubicInOut, duration:1200}}>
        <div id="content">
            {#if page == 0}
            <div id="page" in:slide={{ delay:1000 }} out:slide>
                <h2>Sporcle my beloved,</h2>
            </div>
            {/if}
            {#if page == 1}
            <div id="page" in:slide={{ delay:1000 }} out:slide>
                <table id="table">
                    <tbody>
                        {#each poem_rows as row, i}
                        <tr>
                            <td>{row.letter}</td>
                            <td>
                                {#if visibleRows.has(i)}
                                <div in:fly={{ y:20, duration:600, easing:quintOut }} out:fly={{ y:20, duration:400 }}>
                                    {row.words}
                                </div>
                                {/if}
                            </td>
                        </tr>
                        {/each}
                    </tbody>
                </table>
            </div>
            {/if}
            <div id="buttons">
                {#if page > 0}
                <button class:on={buttonActive == 1} onclick={clickLeft}>
                    <span translate="no" class="material-symbols-outlined">arrow_circle_left</span>
                </button>
                {/if}
                {#if page < pagesEnd}
                <button class:on={buttonActive == 2} onclick={clickRight}>
                    <span translate="no" class="material-symbols-outlined">arrow_circle_right</span>
                </button>
                {/if}
            </div>
        </div>
    </div>
{/if}