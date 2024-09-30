<script>
    import logoSrc from "$lib/assets/new-logo.svg";
    import emailIconSrc from "$lib/assets/sign-up/email-icon.svg";
    import preloaderSrc from "$lib/assets/preloader.svg";
	import { onMount } from "svelte";

    onMount(() => {
        const confirmInstructions = document.getElementById("confirm-instructions");
        const confirmAuthentication = document.getElementById("confirm-authentication");
        const codeInput = document.getElementById("code-input");
        const resendBtn = document.getElementById("resend-btn");

        function isSixDigitNumber(input) {
            return /^\d{6}$/.test(input);
        }

        const switchScreens = () => {
            confirmInstructions.classList.toggle("hide");
            confirmAuthentication.classList.toggle("hide");
        }

        const deleteTokenFromCookies = (name) => {
            document.cookie = name + '=; expires=Thu, 01 Jan 1970 00:00:00 GMT;';
        }

        const authenticate = () => {

            if(isSixDigitNumber(codeInput.value)) {

                const key = codeInput.value;
            
                switchScreens();
            
                fetch("https://task-manager-back-end-7gbe.onrender.com/api/user/confirm", {
                    method: "PATCH",
                    body: JSON.stringify({
                        confirm_key: key
                    }),
                    headers: {
                    "Content-type": "application/json; charset=UTF-8",
                    "verifyUserToken": `Bearer ${getVerifyUserTokenFromCookies()}`
                    }
                })
                .then(res => res.json())
                .then(json => {
                    console.log(json);
                    if(json.status) {
                        const authToken = json.data.authToken;
            
                        const expiryDate = new Date();
                        expiryDate.setMonth(expiryDate.getMonth() + 1);
            
                        document.cookie = `authToken=${authToken}; expires=${expiryDate.toUTCString()}`;

                        deleteTokenFromCookies("verifyUserToken");
            
                        window.location.href = "app";
                    }
                    else {
                        alert(json.message);
                        switchScreens();
                    }
                })
                .catch(err => console.log(err));
            }

            else {
                alert("Confirmation key must be six digits");
            }
        }

        codeInput.addEventListener("keydown", e => {
            if(e.key === "Enter") {authenticate()}
        })

        const transitionBetweenResendAndInput = () => {
            Array.from(confirmInstructions.children).forEach(element => {
                if(element.id != "resend-btn" && element.nodeName != "A") {
                    element.classList.toggle("hide");
                }
            })
        }

        const getVerifyUserTokenFromCookies = () => {
            const tokenCookie = document.cookie.split('; ').find(cookie => cookie.startsWith('verifyUserToken='));
            const verifyUserToken = tokenCookie ? tokenCookie.split('=')[1] : null;
            return verifyUserToken;
        }

        const resendOTP = () => {
            if(!resendBtn.classList.contains("active")) {
                resendBtn.textContent = "Sending...";
                resendBtn.classList.add("active");
                transitionBetweenResendAndInput();
                fetch("https://task-manager-back-end-7gbe.onrender.com/api/user/resendotp", {
                    method: "GET",
                    headers: {
                    "Content-type": "application/json; charset=UTF-8",
                    "verifyUserToken": `Bearer ${getVerifyUserTokenFromCookies()}`
                    }
                })
                .then(res => res.json())
                .then(json => {
                    if(json.status) {
                        resendBtn.textContent = "Code was resent";
                        resendBtn.classList.remove("active");
                        transitionBetweenResendAndInput();
                    }

                    else {
                        alert(json.message);
                    }
                })
                .catch(err => console.log(err));
            }
        }

        resendBtn.addEventListener("click", resendOTP);
    })
</script>

<div class="body-confirmation">
    <img src="{logoSrc}" id="logo" alt="">
    <div id="confirm-instructions">
        <p>Check your email inbox to confirm your account</p>
        <img src="{emailIconSrc}" alt="">
        <input id="code-input" placeholder="Enter the code here..." type="text">
        <p class="confirmation-hint">(Check the spam folder if you don't find the email)</p>
        <button id="resend-btn">Resend code</button>
        <a href="login">Back to the log in page</a>
    </div>
    <div id="confirm-authentication" class="hide">
        <p>Authenticating</p>
        <img src="{preloaderSrc}" alt="">
    </div>
</div>

<style>
    .body-confirmation {
        background-color: black;
        color: white;
        font-family: sans-serif;
        font-weight: 100;
        min-height: 100vh;
        min-height: 100svh;
    }

    .body-confirmation {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .body-confirmation div {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: 1rem;
    }

    .body-confirmation .confirmation-hint {
        color: #777777;
        font-size: .9rem;
        margin-block-start: 2rem;
    }

    .body-confirmation a {
        color: white;
    }

    .body-confirmation input {
        background-color: transparent;
        color: white;
        padding-inline: .5rem;
        border: 1px solid white;
    }

    .body-confirmation button {
        background-color: black;
        color: white;
        border: 1px solid white;
        cursor: pointer;
    }
</style>