<script>
    import logoSrc from "$lib/assets/new-logo.svg";
    import emailIconSrc from "$lib/assets/sign-up/email-icon.svg";
    import visibleSrc from "$lib/assets/sign-up/password-visible-icon.svg";
    import visibleOffSrc from "$lib/assets/sign-up/password-visible-off-icon.svg";
    import nameIconSrc from "$lib/assets/sign-up/name-icon.svg"
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

        // JWT (Token) decryption
        function parseJwt (token) {
            var base64Url = token.split('.')[1];
            var base64 = base64Url.replace(/-/g, '+').replace(/_/g, '/');
            var jsonPayload = decodeURIComponent(window.atob(base64).split('').map(function(c) {
                return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
            }).join(''));

            return JSON.parse(jsonPayload);
        }

        // Sign up Logic
        const firstNameInput = document.getElementById("first-name");
        const lastNameInput = document.getElementById("last-name");
        const emailInput = document.getElementById("email");
        const passwordInput = document.getElementById("password");

        const insertVerifyUserTokenIntoCookies = verifyUserToken => {
            const expiryDate = new Date();
            expiryDate.setMonth(expiryDate.getMonth() + 1);
            document.cookie = `verifyUserToken=${verifyUserToken}; expires=${expiryDate.toUTCString()}`;
        }

        const signUp = () => {
            const fname = firstNameInput.value;
            const lname = lastNameInput.value;

            const name = [fname, lname].join(" ");
            const email = emailInput.value.toLowerCase();
            const password = passwordInput.value;

            if(password == "") {
                window.alert("Password can't be empty");
            }

            else if(fname == "" || lname == "") {
                window.alert("Please enter your full name");
            }

            else if(email == "") {
                window.alert("Please enter your email");
            }
            
            else {
                fetch("https://task-manager-back-end-7gbe.onrender.com/api/user/register", {
                    method: "POST",
                    body: JSON.stringify({
                    name: name,
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
                        insertVerifyUserTokenIntoCookies(json.data.verifyUserToken);
                        window.location.href = "confirmation";
                    }
                    else {
                        alert(json.message);
                    }
                }).catch(err => console.log(err));
            }
        }

        const signUpBtn = document.getElementById("sign-up-btn");
        signUpBtn.addEventListener("click", signUp);

        emailInput.addEventListener("keyup", e => {
            if(e.key === "Enter") {
                signUp();
            }
        });

        passwordInput.addEventListener("keyup", e => {
            if(e.key === "Enter") {
                signUp();
            }
        });

        firstNameInput.addEventListener("keyup", e => {
            if(e.key === "Enter") {
                signUp();
            }
        });

        lastNameInput.addEventListener("keyup", e => {
            if(e.key === "Enter") {
                signUp();
            }
        });
    })
</script>

<div class="body-signup">
    <img src="{logoSrc}" alt="" id="logo">
    <main class="fs-16">
        <p class="fs-20">Sign up for Task Manager</p>
        <div class="input-field">
            <label for="first-name">First name</label>
            <input type="text" name="first-name" id="first-name">
            <img src="{nameIconSrc}" alt="">
        </div>
        <div class="input-field">
            <label for="last-name">Last name</label>
            <input type="text" name="last-name" id="last-name">
            <img src="{nameIconSrc}" alt="">
        </div>
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
        <button id="sign-up-btn">Sign up</button>
        <p><span>Already have an account?</span><span><a href="login">Log in</a></span></p>
    </main>

    <div class="animated-grid">
        
    </div>
</div>

<style>
    .body-signup {
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

    .body-signup main > .input-field:nth-of-type(1), .body-signup main > .input-field:nth-of-type(2) {
        grid-column: span 1;
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
        .body-signup main {
            width: 90%;
            max-width: 500px;
            translate: -50% -50%;
        }

        .body-signup main p {
            text-align: center;
            justify-content: center; /* This one is for the last p element which contains two spans */
        }
    }
    
    /* To be deleted */
    .body-signup {
        /* --clr-accent: silver; */
        --clr-accent: #A0153E;
    }
</style>