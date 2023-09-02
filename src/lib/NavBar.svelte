<script lang="ts">
    import NavLink from "./NavLink.svelte";
    import { getAuth, onAuthStateChanged } from "firebase/auth";

    const auth = getAuth()
    let analyseRedirect = "/login"
    let modifyRedirect = "/login"

    onAuthStateChanged(auth, (user)=>{
        if (user !== null) {
            console.log(user.uid)

            analyseRedirect = "/analyse"
            modifyRedirect = "/modify"
            // ...
        } else {
            analyseRedirect = "/login"
            modifyRedirect = "/login"
        }
    })


</script>

<div id="container">
    <div id="navbar"></div>
    <div id="link-container">
        <div id="leftside">
            <!-- <h1>hi</h1> -->
            <NavLink text="Home" to='/'></NavLink>
            <NavLink text="Get Started" to={analyseRedirect}></NavLink>
            <NavLink text="Modify" to={modifyRedirect}></NavLink>
        </div>
        <!-- <div id="filler"></div> -->
        <div id="rightside">
            <!-- <h1>hi</h1> -->
            <NavLink text="Log in" to="/login"></NavLink>
            <NavLink text="Sign Up" to="/signup"></NavLink>
        </div>
    </div>
</div>

<style>

    #leftside {
        /* background-color: darkgrey; */
        width: 40%;
        height: 100%;
        z-index: 1;
        position: relative;
        display: flex;
        justify-content: space-around;
        align-items: center;
    }

    
    
    #rightside {
        /* background-color: darkgrey; */
        width: 30%;
        height: 100%;
        z-index: 1;
        position: relative;
        display: flex;
        justify-content: space-around;
        align-items: center;
    }

    #link-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        flex-wrap: wrap;
        position: absolute;
        width: 100%;
        /* height: 100%; */
    }

    #navbar {
        background-color: white;
        width: 100%;
        height: 5.5rem;
        margin: 0;
        border-radius: 5px;
        opacity: 0.27;
        /* z-index: -1; */
        position: absolute;
    }

    #container {
        z-index: -1;
        margin-top: 0;
        width: 100%;
        /* color: red; */
        /* position: relative; */
        /* background-color: green; */
    }

</style>