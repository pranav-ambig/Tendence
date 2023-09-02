<script>
    // import auth from "../App.svelte"
    import {getAuth, createUserWithEmailAndPassword } from "firebase/auth";
    import { useNavigate } from "svelte-navigator";
    import { setDoc, doc, getFirestore} from "firebase/firestore";


    let email = ""
    let password = ""
    let errmsg = ""

    const errorMessageMap = {
        "auth/invalid-email" : "Invalid Email",
        "auth/missing-password" : "Enter a Password",
        "auth/weak-password" : "Password should have atleast 6 characters",
        "auth/email-already-in-use" : "Email already exists"
    }

    const navigate = useNavigate();

    
    const handleSignIn = (event)=>{

        event.preventDefault()
        const auth = getAuth()

        createUserWithEmailAndPassword(auth, email, password)
        .then((userCredential) => {
            // Signed in 
            const user = userCredential.user;
            const db = getFirestore();
            setDoc(doc(db, "Users", user.uid), {subjects : []}
            ).then(()=>{
                navigate("/analyse")
            }).catch((error)=>{
                console.log(error)
            })
            
            
        })
        .catch((error) => {
            const errorCode = error.code;
            const errorMessage = error.message;
            console.log(errorMessage)
            errmsg = errorMessageMap[errorCode] ? errorMessageMap[errorCode] : "Invalid"
            
        });
    }


</script>

<div id="signup-div-main">
    <div id="center-div">
        <div id="style-div"></div>
        <div id="content-div">
            <p id="heading">Create an Account</p>
            <div id="form-div">
                <input type="email" placeholder="Email" id="email-inp" bind:value={email}/>
                <input type="password" placeholder="Password" id="pass-inp" bind:value={password} />
                <p style={errmsg===""?"opacity:0":"opacity:1"} id="err-ind">{errmsg}</p>
                <input 
                    style="text-align: center;"
                    value="Sign Up" 
                    id="submit-btn" 
                    readonly
                    on:click={handleSignIn}/>
            </div>
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <p id="login-redir" on:click={()=>navigate('/login')}><u>Log into an existing account</u></p>
            
        </div>
    </div>

</div>

<style>

#login-redir {
    cursor: pointer;
}

#err-ind {
    height: 1rem;
    margin: 0;
}



#submit-btn {
    transition: all 0.3s ease;
    cursor: pointer;
}

#submit-btn:hover {
    background-color: #E00A55;
    transition: all 0.3s ease;
    color: white;
}

#form-div {
    display: flex;
    /* justify-content: space-between; */
    position: relative;
    align-items: center;
    flex-direction: column;
    /* height: 90%; */
    /* height: 30%; */
}

input {
    margin: 1.2rem;
    font-size: 1.2rem;
    min-width: 16rem;
    width: fit-content;
    padding: 0.5rem;
    padding-left: 1rem;
    border-radius: 2rem;
    outline: none;
    border: none;
}

#heading {
    font-size: 1.8rem;
    margin: 0.5rem;
    /* margin-top: 2rem; */
}

#content-div {
    /* position: absolute; */
    width: 27rem;
    height: 27rem;
    /* background-color: green; */
    z-index: 2;
    opacity: 1;
    color: white;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    
}

#style-div {
    position: absolute;
    z-index: -1;
    width: 27rem;
    height: 27rem;
    opacity: 0.27;
    background-color: white;
    border-radius: 5px;
}


#center-div {
    /* background-color: white; */
    width: 27rem;
    height: 27rem;
    margin-top: 10vh;
    border-radius: 5px;
    /* opacity: 0.27; */
    /* position: absolute; */
    /* z-index: -1; */

}

#signup-div-main {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 90vh;
}

</style>