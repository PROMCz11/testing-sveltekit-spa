<script>
    import logoSrc from "$lib/assets/new-logo.svg";
    import emailIconSrc from "$lib/assets/sign-up/email-icon.svg";
    import visibleSrc from "$lib/assets/sign-up/password-visible-icon.svg";
    import visibleOffSrc from "$lib/assets/sign-up/password-visible-off-icon.svg";
	import { onMount } from "svelte";

    onMount(() => {
        const inputs = document.querySelectorAll(".input-field input");

        inputs.forEach(input => {
            input.addEventListener("focus", () => {
                if(!input.parentElement.querySelector("label").classList.contains("shift-label")) {
                    input.parentElement.querySelector("label").classList.toggle("shift-label");
                }
            });
            
            input.addEventListener("blur", () => {
                if(!input.value) {
                    input.parentElement.querySelector("label").classList.toggle("shift-label");
                }
            });
        });
    
        const visibilityBtnOn = document.getElementById("visibility-btn-on");
        const visibilityBtnOff = document.getElementById("visibility-btn-off");
        
        visibilityBtnOn.addEventListener("click", () => {
            visibilityBtnOn.classList.toggle("hide");
            visibilityBtnOff.classList.toggle("hide");
            visibilityBtnOn.parentElement.querySelector("input#password").type = "text";
        });
        
        visibilityBtnOff.addEventListener("click", () => {
            visibilityBtnOn.classList.toggle("hide");
            visibilityBtnOff.classList.toggle("hide");
            visibilityBtnOff.parentElement.querySelector("input#password").type = "password";
        });
    
        // Grid logic
        const animatedGrid = document.querySelector('.animated-grid');
        const boxNumber = 900;
        let boxesHTML;
        for (let i = 0; i < boxNumber; i++) {
            boxesHTML += '<div class="box"></div>';
        }
        animatedGrid.innerHTML += boxesHTML;
        const minDelay = 0;
        const maxDelay = 10000;
        const animatedGridBoxes = document.querySelectorAll('.animated-grid .box');
        animatedGridBoxes.forEach(box => {
            box.classList.toggle('glow');
            const classAssignmentDelay = Math.floor(Math.random() * (maxDelay - minDelay + 1)) + minDelay;
            box.style.animationDelay = `${classAssignmentDelay}ms`;
            const selectedColor = Math.floor(Math.random() * 16) + 1;
            setTimeout(() => {
                if(selectedColor === 1 || selectedColor === 2 || selectedColor === 3) {
                    box.classList.add('box-glow');
                }
            }, classAssignmentDelay);
        });
    
        // Log in Logic
        const emailInput = document.getElementById("email");
        const passwordInput = document.getElementById("password");
        
        const insertAuthTokenIntoCookies = authToken => {
            const expiryDate = new Date();
            expiryDate.setMonth(expiryDate.getMonth() + 1);
            document.cookie = `authToken=${authToken}; expires=${expiryDate.toUTCString()}`;
        }
        
        const logIn = () => {
            const email = emailInput.value.toLowerCase();
            const password = passwordInput.value;
            fetch("https://task-manager-back-end-7gbe.onrender.com/api/user/login", {
                method: "POST",
                body: JSON.stringify({
                email: email,
                password: password
                }),
                headers: {
                "Content-type": "application/json; charset=UTF-8"
                }
            })
            .then(res => res.json())
            .then(json => {
                if(json.status) {
                    const authToken = json.data.authToken;
                    insertAuthTokenIntoCookies(authToken);
                    window.location.href = "app";
                }
                else {
                    // alert("Email or Password is incorrect");
                    alert(json.message);
                }
            }).catch(err => console.log(err))
        }
        
        const logInBtn = document.getElementById("log-in-btn");
        logInBtn.addEventListener("click", logIn);
        
        emailInput.addEventListener("keyup", e => {
            if(e.key === "Enter") {
                logIn();
            }
        });
        passwordInput.addEventListener("keyup", e => {
            if(e.key === "Enter") {
                logIn();
            }
        });
    })
</script>

<div class="body-login">
    <img src="{logoSrc}" alt="" id="logo">
    <main class="fs-16">
        <p class="fs-20">Log in to Task Manager</p>
        <div class="input-field">
            <label for="email">Email</label>
            <input type="email" name="email" id="email">
            <img src="{emailIconSrc}" alt="">
        </div>
        <div class="input-field">
            <label for="password">Password</label>
            <input type="password" name="password" id="password">
            <img id="visibility-btn-on" src="{visibleSrc}" alt="">
            <img class="hide" id="visibility-btn-off" src="{visibleOffSrc}" alt="">
        </div>
        <div class="remember-me-wrapper"><input type="checkbox" name="remember-me" id="remember-me"><label for="remember-me">Remember me</label></div>
        <button id="log-in-btn">Log in</button>
        <p><span>You don't have an account?</span><span><a href="signup">Sign up</a></span></p>
    </main>

    <div class="animated-grid">
        
    </div>
</div>

<style>
    .body-login {
        background-color: black;
        /* font-size: 1.2rem; */
        color: white;
        font-family: sans-serif;
        font-weight: 100;
    }

    main {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 2rem;
        width: 30vw;
        position: absolute;
        top: 50%;
        left: 50%;
        translate: -120% -50%;
        z-index: 1;
    }

    main > p {
        grid-column: span 2;
    }

    main > p:last-of-type {
        display: flex;
        gap: 1rem;
        font-size: 1rem;
    }

    main > p:last-of-type a {
        color: white;
    }

    main > .input-field {
        grid-column: span 2;
    }

    @media (max-width: 1200px) {
        .body-login main {
            width: 90%;
            max-width: 500px;
            translate: -50% -50%;
        }

        .body-login main p {
            text-align: center;
            justify-content: center; /* This one is for the last p element which contains two spans */
        }
    }
    
    /* To be deleted */
    .body-login {
        /* --clr-accent: silver; */
        --clr-accent: #A0153E;
    }
</style>