<script lang="ts">
    import { useNavigate } from "svelte-navigator";
    import {getAuth, signOut } from "firebase/auth";

    const navigate = useNavigate();
    const auth = getAuth();
    signOut(auth)
    .then(()=>{
        console.log('Logged out successfully')
        navigate('/login')
    })
    .catch((e)=>console.log(e))

</script>