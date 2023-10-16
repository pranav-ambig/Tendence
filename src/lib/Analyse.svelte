<script>
    import { doc, setDoc, getDoc } from "firebase/firestore"; 
    import { getAuth } from "firebase/auth";
    import { getFirestore } from "firebase/firestore";
    import { useNavigate } from "svelte-navigator";
    import { writable } from "svelte/store";
    // import currentSubData from "./Modify.svelte";

    const auth = getAuth()
    const user = auth.currentUser
    const db = getFirestore()
    const navigate = useNavigate()

    let subjects = writable([])
    let attendance = writable([])

    // console.log(user.uid);

    let currentSubData = {
        "subjects" : []
    }

    let threshold = 0.8;


    // console.log(user)
    if (user === null){
        navigate('/login')
    }
    else {
        let docRef = doc(db, "Users", user.uid)
        getDoc(docRef)
        .then((docSnap)=>{
            // let rawData = docSnap.data();
            // @ts-ignore
            currentSubData = docSnap.data();
            // console.log(currentSubData)

            // let tempSub = []
            // let tempAtten = []
            // rawData.subjects.forEach((subject)=>{
            //     tempSub.push(subject.name)
            //     tempAtten.push([subject.current, subject.total])
            // })
            // subjects.set(tempSub)
            // attendance.set(tempAtten)
            // // console.log(subjects)
    
        })
        .catch((error)=>{
            console.log(error)
        })
    }

    let status = "0"
    let status_text;
    if (status == "0"){
        status_text = "Your attendence is all Good!"
    }

    let makeUpMode = true;

    const calculateMakeup = (current, total) => {
        if (current/total > threshold)
            return 0; 
        let d = 0;
        while ((current+d)/(total+d) < threshold){
            d += 1;
        }
        return d;
    }

    const calculateLeave = (current, total) =>{
        let d = 0;
        while (current/(total+d) > threshold) d += 1;
        return d;
    }

    // const chooseMakeLeave = (current, total)=>{
    //     if (makeUpMode)
    //         return calculateMakeup(current, total)
    //     else
    //         return calculateLeave(current, total)
    // }


</script>

<div id="analyse-div-main">
    {#if currentSubData["subjects"].length == 0}
        <div id="no-sub-div">
            <p id="no-sub">Add some subjects to get started!</p>
        </div>
    {:else}
        <table>
            <tr>
                <td>Subject</td>
                <td>Current</td>
                <td>Total</td>
                <td>Percentage</td>
                <!-- svelte-ignore a11y-click-events-have-key-events -->
                <td 
                on:click={()=>{makeUpMode = !makeUpMode}}
                style="cursor:pointer"
                >{makeUpMode?"Makeup":"Leave"}</td>
            </tr>
            {#each currentSubData["subjects"] as subject}
                <tr>
                    <td>{subject.name}</td>
                    <td>{subject.current}</td>
                    <td>{subject.total}</td>
                    <td>{Math.round(subject.current*10000/subject.total)/100}%</td>
                    {#if makeUpMode}
                        <td>{calculateMakeup(subject.current, subject.total)}</td>
                    {:else}
                        <td>{calculateLeave(subject.current, subject.total)}</td>
                    {/if}
                </tr>
            {/each}
        </table>
        <div id="analysis-ctn">
            <p id="analysis-tag">Quick Look</p>
            <p id="status-summary">
                {status_text}
            </p>
        </div>
    {/if}
    
    
</div>


<style>

    #no-sub-div {
        height: 80vh;
        display: flex;
        justify-content: center;
        flex-direction: column;
    }

    #no-sub {
        font-size: 2rem;
        color: white;
        /* margin-top: 12rem; */
    }

    #status-summary {
        color: white;
        /* font-weight: 300; */
        font-size: 2.5rem;
        text-align: center;
    }

    #analysis-ctn {
        width: 90vw;
        height: 90vh;
        background-color: #ffffff33;
        margin: auto;
        border-radius: 7px;
    }

    #analysis-tag {
        font-size: 3rem;
        color: white;
        text-align: center;
        position: relative;
        /* padding: 3rem; */
        /* margin-top: 100px; */
    }

    tr {
        height: 3rem;
    }
    
    td {
        /* border: 1px solid; */
        border-radius: 7px;
        text-align: center;
        background-color: #ffffff33;
        width: 10vw;
        padding-top: 1rem;
        padding-bottom: 1rem;
        transition: all 0.4s ease;

    }

    td:hover {
        /* border: 1px solid; */
        border-radius: 7px;
        text-align: center;
        background-color: #ffffff88;
        width: 10vw;
        transition: all 0.4s ease;
    }

    table {
        width: 80vw;
        min-height: 30rem;
        /* height: fit-content; */
        /* margin-top: 8vh; */
        
        color: white;
        /* border: 1px solid; */
        border-radius: 10px;
        padding: 20px;
        font-size: 1.5rem;
        outline: #ffffff33;
        /* grid-gap: 10rem; */
        /* position: relative; */
    }

    #analyse-div-main {
        /* margin-top: 5.5rem; */
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        /* flex: 1; */
        position: relative;
        /* height: 80vh; */
    }

</style>
