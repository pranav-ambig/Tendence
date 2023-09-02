<script>
    import Button from './Button.svelte';
    

</script>

<div id="container">
  <h1 id="heading">Tendence</h1>
  <p id="tagline">Some quick attendance analysis?</p>
  
  <div id="letsgo-btn-container">
      <Button text="Let's Go!" ></Button>
  </div>
</div>

<style>
    #container {
      height: 100%;
    }
  
    #heading {
    color: white;
    font-weight: 350;
    letter-spacing: .8rem;
    font-size: 5rem;
    width: 100%;
    text-align: center;
    margin-top: 8rem;
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
    margin-top: 2vh;
  }

  #letsgo-btn-container {
    margin-top: 3vh;
    display: flex;
    justify-content: center;
    position: relative;
  }
</style>
