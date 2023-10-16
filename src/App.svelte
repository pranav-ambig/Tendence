<script lang="ts">
  import svelteLogo from './assets/svelte.svg'
  import Counter from './lib/Counter.svelte'
  import NavLink from './lib/NavLink.svelte';
  import NavBar from './lib/NavBar.svelte';
  import Button from './lib/Button.svelte';

  import {Route, Router, Link, useNavigate} from 'svelte-navigator'
  import Analyse from './lib/Analyse.svelte';
  import Modify from './lib/Modify.svelte';
  import SignUp from './lib/SignUp.svelte';
  import LogIn from './lib/LogIn.svelte';
  import Home from './lib/Home.svelte';

  // Import the functions you need from the SDKs you need
  import { initializeApp } from "firebase/app";
  import { getAuth, connectAuthEmulator } from "firebase/auth";
  import { getFirestore, connectFirestoreEmulator  } from "firebase/firestore";
    import LogOut from './lib/LogOut.svelte';
  // import { getAnalytics } from "firebase/analytics";
  // TODO: Add SDKs for Firebase products that you want to use
  // https://firebase.google.com/docs/web/setup#available-libraries

  // Your web app's Firebase configuration
  // For Firebase JS SDK v7.20.0 and later, measurementId is optional
  const firebaseConfig = {
    apiKey: "AIzaSyDM8StNzXjygKOJqAZmDYcopUyN1-7M1Z8",
    authDomain: "tendence-23329.firebaseapp.com",
    projectId: "tendence-23329",
    storageBucket: "tendence-23329.appspot.com",
    messagingSenderId: "849269992129",
    appId: "1:849269992129:web:12f002469e0d0c712a4240",
    measurementId: "G-JVSPVFY31L"
  };

  // Initialize Firebase
  const app = initializeApp(firebaseConfig);
  export const auth = getAuth(app);
  export const db = getFirestore(app);
  // connectFirestoreEmulator(db, '127.0.0.1', 8081);
  // connectAuthEmulator(auth, "http://127.0.0.1:9099");
  // const analytics = getAnalytics(app);


</script>

<Router>

  
  <div id="main-container">
    <div id="bg-image">
    </div>
    <!-- <video autoplay muted loop controls id="bg-image">
      <source src="assets\BG.mp4" type="video/mp4"/>
    </video> -->
  
  
    <!-- <NavLink text="Home"></NavLink> -->
    <div id="navbar-container">
      <NavBar></NavBar>
    </div>

    <Route path='/'>
      <!-- <h1 id="heading">Tendence</h1>
      <p id="tagline">Some quick attendance analysis?</p>
    
      <div id="letsgo-btn-container">
        <Button text="Let's Go!" ></Button>
      </div> -->
      <Home></Home>
    </Route>

    <Route path="analyse">
      <Analyse></Analyse>
    </Route>

    <Route path="modify">
      <Modify></Modify>
    </Route>

    <Route path="login">
      <LogIn></LogIn>
    </Route>

    <Route path="signup">
      <SignUp></SignUp>
    </Route>

    <Route path="logout">
      <LogOut></LogOut>
    </Route>
  
  </div>
  

  

</Router>

<style>

  /* :focus {
    outline: none;
  } */
  
  #main-container {
    height: 100%;
    position: relative;
    /* display: flex;
    flex-direction: column; */
  }

  #navbar-container {
    position: relative;
    width: 100%;
    top: 0%;
    height: 5.5rem;
    /* z-index: 0; */
    /* height: 7vh; */
  }

  

  #bg-image { 
    background: url(./assets/bg-image-org.jpg) no-repeat center center fixed; 
    -webkit-background-size: cover;
    -moz-background-size: cover;
    -o-background-size: cover;
    background-size: cover;
    width: 100%;
    min-height: 100%;
    top: 0;
    position: fixed;
    filter: blur(40px);
    z-index: -1;
    transform: scale(1.07);
}

</style>