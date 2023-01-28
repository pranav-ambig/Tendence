<script lang="ts">
  import svelteLogo from './assets/svelte.svg'
  import Counter from './lib/Counter.svelte'
  import NavLink from './lib/NavLink.svelte';
  import NavBar from './lib/NavBar.svelte';
  import Button from './lib/Button.svelte';
</script>

<div id="main-container">
  <div id="bg-image">
  </div>


  <!-- <NavLink text="Home"></NavLink> -->
  <div id="navbar-container">
    <NavBar></NavBar>
  </div>
  <h1 id="heading">Tendence</h1>
  <p id="tagline">Some quick attendance analysis?</p>

  <div id="letsgo-btn-container">
    <Button text="Let's Go!"></Button>
  </div>

</div>

<style>
  #navbar-container {
    position: relative;
    width: 100%;
    top: 0%;
    height: 7rem;
  }

  #heading {
    color: white;
    font-weight: 350;
    letter-spacing: .8rem;
    font-size: 5rem;
    width: 100%;
    text-align: center;
    margin-top: 7rem;
    /* top: 50%; */
    margin-bottom: 0;

    position: relative;
  }

  #tagline {
    color: white;
    font-weight: 350;
    letter-spacing: .2rem;
    font-size: 1.6rem;
    
    text-align: center;
    margin-top: 2%;
  }

  #letsgo-btn-container {
    margin-top: 3rem;
    display: flex;
    justify-content: center;
    position: relative;
  }



  /* #main-container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    flex-direction: row;
  } */

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