<script>
    import workflowIconSrc from "$lib/assets/home/workflow-icon.svg";
    import prioritizeIconSrc from "$lib/assets/home/prioritize-icon.svg";
    import connectedIconSrc from "$lib/assets/home/connected-icon.svg";
    import fortressIconSrc from "$lib/assets/home/fortress-icon.svg";

    import { animate, inView, stagger } from "motion";
	import { onMount } from "svelte";

    onMount(() => {
        document.querySelector(".loading-screen").classList.add("hide");

        animate(".box-grid", {gap: ["10rem", "1rem"], opacity: [0, 1]}, {duration: .7});
        animate(".hero p", {opacity: [0, 1], scale: [1.5, 1]}, {duration: .7});

        inView(".general-features", () => {
            animate(".box-grid", {gap: ["1rem", "5rem"], opacity: [1, 0], display: ["grid", "none"]}, {duration: .7});
            animate(".hero p", {opacity: [1, 0], scale: [1, 1.5], display: ["block", "none"]}, {duration: .7});

            return () => {
                animate(".box-grid", {gap: ["5rem", "1rem"], opacity: [0, 1], display: ["none", "grid"]}, {duration: .7});
                animate(".hero p", {opacity: [0, 1], scale: [1.5, 1], display: ["none", "block"]}, {duration: .7});
            }
        })

        inView(".general-features", () => {
            animate(".general-features > div", {scale: [.8, 1], opacity: [0, 1], translate: ["-30%", "0"]}, {delay: stagger(0.2)});
        })
    })
</script>

<div class="body-home">
    <section class="hero">
        <p>The <span>Future</span> of Task Management<br>Starts <span>Here</span></p>
        <div class="box-grid">
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
        </div>
    </section>
    <section class="general-features">
        <div>
            <p>Seamless Workflow</p>
            <p>Manage tasks effortlessly, reducing clutter and streamlining your day-to-day operations with intuitive automation.</p>
            <div>
                <p>Seamless Workflow</p>
                <img src="{workflowIconSrc}" alt="">
            </div>
        </div>
        <div>
            <p>Prioritize Like a Pro</p>
            <p>Mark critical tasks as important with one click to stay on top of what matters most.</p>
            <div>
                <p>Prioritize Like a Pro</p>
                <img src="{prioritizeIconSrc}" alt="">
            </div>
        </div>
        <div>
            <p>Stay Connected</p>
            <p>Access tasks on any device, from desktop to mobile, with smooth synchronization in real-time.</p>
            <div>
                <p>Stay Connected</p>
                <img src="{connectedIconSrc}" alt="">
            </div>
        </div>
        <div>
            <p>Fortress of Data</p>
            <p>Enjoy peace of mind with state-of-the-art data encryption and security protocols to protect your most sensitive information.</p>
            <div>
                <p>Fortress of Data</p>
                <img src="{fortressIconSrc}" alt="">
            </div>
        </div>
    </section>

    <div class="loading-screen">
        Loading...
    </div>
    <div class="disclamer" style="padding: 1rem;">
        <p>The home page is currently under development</p>
        <p>Please proceed to the rest of the application:</p>
        <p><span><a style="color: white;" href="login">Login</a></span> <span><a style="color: white;" href="signup">Signup</a></span></p>
    </div>
</div>

<style>
    .loading-screen {
        position: fixed;
        inset: 0;
        display: flex;
        justify-content: center;
        align-items: center;
        z-index: 100;
        background-color: black;
    }

    .hide {
        display: none;
    }

    .general-features {
        min-height: 100vh;
        display: flex;
        justify-content: center;
        gap: 18px;
        flex-wrap: wrap;
    }

    .general-features > div {
        width: 280px;
        height: 360px;
        padding: 1rem;
        font-size: 20px;
        border: #222222 8px solid;
        border-radius: 10px;
        position: relative;
        overflow: hidden;
        transition: scale 200ms;

        opacity: 0;
    }

    .general-features > div > p:first-of-type {
        margin-bottom: 44px;
        font-size: 24px;
    }

    .general-features > div > div {
        position: absolute;
        inset: 0;
        background-color: #222222;
        padding: 1rem;
        transition: translate 200ms;
        font-size: 24px;
        text-align: center;
        display: flex;
        flex-direction: column;
        gap: 90px;
        align-items: center;
        justify-content: center;
    }

    .general-features > div:hover {
        scale: 1.05;
    }

    .general-features > div:hover > div {
        translate: 0 -100%;
    }

    .body-home {
        min-height: 100vh;
        min-height: 100svh;
        --clr-accent: #A0153E;
        font-family: sans-serif;
    }

    .hero {
        min-height: 120vh;
        max-width: 100vw;
        overflow: hidden;
        position: relative;
    }

    .hero span {
        color: var(--clr-accent);
    }

    .hero p {
        width: 100%;
        position: fixed;
        top: 50%;
        left: 50%;
        translate: -50% -50%;
        text-align: center;
        font-size: 4rem;
        z-index: 2;
    }

    .box {
        width: 100px;
        height: 100px;
        border-radius: 10px;
        background-color: #1a1a1a;
    }

    .box-grid {
        display: grid;
        grid-template-columns: repeat(8, 1fr);
        gap: 10rem;
        position: fixed;
        top: 50%;
        left: 50%;
        translate: -50% -50%;
    }

    @media (max-width: 1000px) {
        .hero p {
            font-size: 2.5rem;
        }
    }
</style>