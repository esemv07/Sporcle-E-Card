<script>
    import { onMount } from "svelte";
    import { cubicInOut, quintOut } from "svelte/easing";
    import { draw, fly, scale, slide } from "svelte/transition";

    import map from "$lib/assets/world-map.jpeg";
    import heart from "$lib/assets/heart.svg";
    import globe from "$lib/assets/globe.svg";
    import flag from "$lib/assets/country-flag.svg";
    import atlas from "$lib/assets/map-book.svg";
    import foldmap from "$lib/assets/map-icon.svg";
    import searchmap from "$lib/assets/search-map.svg";



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
    let pagesEnd = 6;

    let buttonActive = $state(0);

    let visibleRows = $state(new Set());
    let ansColour = $state('white');

    function addRows() {
        poem_rows.forEach((_, i) => {
            setTimeout(() => {
                visibleRows = new Set(visibleRows).add(i);
            }, i * 1500)
        }); 
    };

    function clickLeft() {
        if (page == 2) {
            visibleRows = new Set();
            page--;
            setTimeout(addRows, 1600);
        } else if (page == 5 || page == 4) {
            ansColour = 'white';
            setTimeout(() => {
                ansColour = '#4faf46';
            }, 2000)
            page--;
        } else {
            page--;
        }
    };

    function clickRight() {
        if (page == 0) {
            visibleRows = new Set();
            page++;
            setTimeout(addRows, 1600);
        } else if (page == 2 || page == 3) {
            ansColour = 'white';
            setTimeout(() => {
                ansColour = '#4faf46';
            }, 2000)
            page++;
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
        text-align: center;
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

    #map-container {
        position: relative;
    }

    img {
        height: 100%;
        margin-bottom: 30px;
        display: block;
    }

    #country-text {
        position: absolute;
        background-color: #1d5736;
        color: white;
        border-radius: 5px;
        padding: 5px;
        top: 50%;
        left: 50%;
    }

    #anstable {
        width: 400px;
        height: 150px;
        border-spacing: 20px;
        margin-left: auto;
        margin-right: auto;
    }

    #anstable td {
        width: 50%;
        text-align: center;
        background-color: white;
    }

    #imgtable {
        width: 250px;
        height: 300px;
        border-collapse: collapse;
        margin-left: auto;
        margin-right: auto;
        background-color: white;
    }

    #imgtable td {
        text-align: center;
        align-items: center;
        justify-content: center;
    }
</style>

{#if toggle}
    <div id="card" transition:fly={{y:-200, easing: cubicInOut, duration:1200}}>
        <div id="content">
            {#if page == 0}
            <div id="page" in:slide={{ delay:1000 }} out:slide>
                <h2>Sporcle my beloved,</h2>
                <img src={globe} width="90px" height="auto" style="margin-left: auto; margin-right: auto;" alt="globe" />
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
                <img src={foldmap} width="150px" height="auto" style="position: fixed; bottom: 0px; left: 400px; transform: rotate(20deg);" alt="map" />
                <img src={flag} width="150px" height="auto" style="position: fixed; bottom: 0px; right: 400px; transform: rotate(-20deg);" alt="map" />
            </div>
            {/if}
            {#if page == 2}
            <div id="page" in:slide={{ delay:1000 }} out:slide>
                <div id="map-container">
                    <img src={map} alt="World Map" />
                    <p id="country-text" style="transform: translate(-350px, -225px)" in:fly={{ y:20, delay:1000, duration:600, easing:quintOut }} out:fly={{ y:20, duration:400 }}>you</p>
                    <p id="country-text" style="transform: translate(-275px, 25px)" in:fly={{ y:20, delay:1800, duration:600, easing:quintOut }} out:fly={{ y:20, duration:400 }}>taught</p>
                    <p id="country-text" style="transform: translate(-90px, -160px)" in:fly={{ y:20, delay:2600, duration:600, easing:quintOut }} out:fly={{ y:20, duration:400 }}>me</p>
                    <p id="country-text" style="transform: translate(-25px, -110px)" in:fly={{ y:20, delay:3400, duration:600, easing:quintOut }} out:fly={{ y:20, duration:400 }}>my</p>
                    <p id="country-text" style="transform: translate(150px, -110px)" in:fly={{ y:20, delay:4200, duration:600, easing:quintOut }} out:fly={{ y:20, duration:400 }}>way</p>
                    <p id="country-text" style="transform: translate(250px, -65px)" in:fly={{ y:20, delay:5000, duration:600, easing:quintOut }} out:fly={{ y:20, duration:400 }}>in</p>
                    <p id="country-text" style="transform: translate(290px, 5px)" in:fly={{ y:20, delay:5800, duration:600, easing:quintOut }} out:fly={{ y:20, duration:400 }}>the</p>
                    <p id="country-text" style="transform: translate(325px, 120px)" in:fly={{ y:20, delay:6600, duration:600, easing:quintOut }} out:fly={{ y:20, duration:400 }}>world</p>
                </div>
            </div>
            {/if}
            {#if page == 3}
            <div id="page" in:slide={{ delay:1000 }} out:slide>
                <h2>Together we travelled the...</h2>
                <table id="anstable">
                    <tbody>
                        <tr>
                            <td>Country</td>
                            <td style:background-color={ansColour}>World</td>
                        </tr>
                        <tr>
                            <td>Road</td>
                            <td>Ocean</td>
                        </tr>
                    </tbody>
                </table>
                <img src={searchmap} width="80px" height="auto" style="margin-top: 20px; margin-left: auto; margin-right: auto; transform: rotate(-20deg)" alt="search map" />
            </div>
            {/if}
            {#if page == 4}
            <div id="page" in:slide={{ delay:1000 }} out:slide>
                <h2>My gratitude to you is...</h2>
                <table id="anstable">
                    <tbody>
                        <tr>
                            <td style:background-color={ansColour}>Immense</td>
                            <td>None</td>
                        </tr>
                        <tr>
                            <td>What gratitude?</td>
                            <td>Small</td>
                        </tr>
                    </tbody>
                </table>
                <img src={atlas} width="80px" height="auto" style="margin-top: 20px; margin-left: auto; margin-right: auto; transform: rotate(-20deg)" alt="atlas" />
            </div>
            {/if}
            {#if page == 5}
            <div id="page" in:slide={{ delay:1000 }} out:slide>
                <h2>So now all there is to say is...</h2>
                <table id="imgtable">
                    <tbody>
                        <tr>
                            <td>
                                <img style:margin="auto" src={heart} alt="Heart" width="90%" height="auto"/>
                            </td>
                        </tr>
                        <tr>
                            <td in:fly={{ y:20, delay:2000, duration:600, easing:quintOut }} out:fly={{ y:20, duration:400 }}>THANKS!</td>
                        </tr>
                    </tbody>
                </table>
            </div>
            {/if}
            {#if page == 6}
            <div id="page" in:slide={{ delay:1000 }} out:slide>
                <h2>Love esemv &lt;3</h2>
                <img src={flag} width="60px" height="auto" style="margin-left: auto; margin-right: auto; transform: rotate(20deg)" alt="flag" />
            </div>
            {/if}
            <div id="buttons">
                {#if page > 0}
                <button class:on={buttonActive == 1} onclick={clickLeft}>
                    <span translate="no" class="material-symbols-outlined">keyboard_double_arrow_left</span>
                </button>
                {/if}
                {#if page < pagesEnd}
                <button class:on={buttonActive == 2} onclick={clickRight}>
                    <span translate="no" class="material-symbols-outlined">keyboard_double_arrow_right</span>
                </button>
                {/if}
            </div>
        </div>
    </div>
{/if}