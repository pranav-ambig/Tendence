<script lang="ts">

    import { setDoc, doc, getFirestore, getDoc } from "firebase/firestore";
    import { getAuth } from "firebase/auth";

    const auth = getAuth();
    const user = auth.currentUser;
    const db = getFirestore();

    //TODO
    // make second input flash red if its value is smaller than first input

    let addSubModalVisible = false;
    let editMode = false;

    let addSubObj = {
        name: "",
        current : null,
        total : null,
        classes : new Array(7).fill(null)
    }

    const initAddSubObj = () => {
        addSubObj = {
            name: "",
            current : null,
            total : null,
            classes : new Array(7).fill(null)
        }
    }

    let currentSubData = {
        "subjects" : []
    };

    //fetches subjects data from db
    let docRef = doc(db, "Users", user.uid)
    getDoc(docRef)
    .then((docSnap)=>{
        //@ts-ignore
        currentSubData = docSnap.data();
        // if (currentSubData["subjects"] == undefined){
        //     console.log('afjksfd')
        // }
        // else {
        //     console.log('nope')
        // }
    })
    .catch((error)=>{
        console.log(error)
    })


    const mainDivClickHandler = (event) => {
        
    }

    const addSubClickHandler = () => {
        initAddSubObj()
        addSubModalVisible = !addSubModalVisible
        // console.log('sodfjadfnk')
    }

    const editSubClickHandler = (subject) => {
        addSubObj = subject
        addSubModalVisible = true;
        editMode = true;
    }

    const deleteSubClickHandler = (subject) => {
        let temp = currentSubData;
        let i = temp["subjects"].indexOf(subject)
        temp["subjects"].splice(i, 1)
        currentSubData = temp
        editMode = true;
        handleSave()
        addSubModalVisible = false;
    }

    const handleSave = () => {
        if (addSubObj.current > addSubObj.total){
            addSubObj.current = addSubObj.total
        }
        let temp = currentSubData;
        if (temp === undefined)
            temp = {subjects : [addSubObj]}
        else{
            if (!editMode)
                temp["subjects"].push(addSubObj);
            editMode = false
        }


        currentSubData = temp;
        setDoc(doc(db, "Users", user.uid), currentSubData
        ).then(()=>{
            // console.log("uploaded successfully")
        }).catch((error)=>{
            console.log(error)
        })
        initAddSubObj()
        addSubClickHandler();
    }

    const handleOutClick = () => {
        if (addSubModalVisible)
            addSubModalVisible = false;
    }

</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div id="modify-div-main" on:click={mainDivClickHandler}>
    <table id="table" class:blur = {addSubModalVisible} class="unblurred">
        <tr>
            <td>Subject</td>
            <td>Current</td>
            <td>Total</td>
            {#if currentSubData.subjects.length}
            <td colspan="2" class="action">Actions</td>
                
            {/if}
        </tr>
        {#if currentSubData !== undefined}
            {#each currentSubData["subjects"] as subject}
                    <tr>
                        <td>{subject.name}</td>
                        <td>{subject.current}</td>
                        <td>{subject.total}</td>
                        <td class="action" on:click={()=>editSubClickHandler(subject)}>Edit</td>
                        <td class="action" on:click={()=>deleteSubClickHandler(subject)}>Delete</td>
                    </tr>
            {/each}
        {/if}
        <tr>
            <td colspan="3" id="add-sub" on:click={addSubClickHandler}>Add subject</td>
        </tr>
    </table>
    {#if addSubModalVisible}
        <div id="add-sub-modal">
            <div id="modal-content">
                    <p id="add-sub-title">Add Subject</p>
                    <input type="text" placeholder="Subject Name" bind:value={addSubObj.name}/>
                    <input type="number" placeholder="Attended Classes" bind:value={addSubObj.current}/>
                    <input type="number" class="normal" placeholder="Total Classes" bind:value={addSubObj.total}/>
                    <p>Classes each day</p>
                    <div id="time-table-editor">
                        <input type="number" min="0" placeholder="Mon" bind:value={addSubObj.classes[0]}/>
                        <input type="number" min="0" placeholder="Tue" bind:value={addSubObj.classes[1]}/>
                        <input type="number" min="0" placeholder="Wed" bind:value={addSubObj.classes[2]}/>
                        <input type="number" min="0" placeholder="Thu" bind:value={addSubObj.classes[3]}/>
                        <input type="number" min="0" placeholder="Fri" bind:value={addSubObj.classes[4]}/>
                        <input type="number" min="0" placeholder="Sat" bind:value={addSubObj.classes[5]}/>
                        <input type="number" min="0" placeholder="Sun" bind:value={addSubObj.classes[6]}/>
                    </div>
                    <div></div>
                    <input 
                        id="save-sub-btn"
                        value="Save"
                        readonly
                        on:click={handleSave}
                        />
            </div>
        </div>
    {/if}
    <div id="footer-div">

    </div>
</div>

<style>

.action {
    max-width: 3rem;
    cursor: pointer;
}

#footer-div {
    width: 100%;
    height: 2rem;
}

#save-sub-btn {
    transition: all 0.3s ease;
    cursor: pointer;
    text-align: center;
}

#save-sub-btn:hover {
    background-color: #E00A55;
    transition: all 0.3s ease;
    color: white;
}

#time-table-editor > input {
    position: relative;
    width: 10%;
    height: 1.5rem;
    border-radius: 10rem;
    outline: none;
    border: none;
    padding: 0.5rem;
    text-align: center;
}

#time-table-editor {
    display: flex;
    width: 100%;
    justify-content: space-evenly;
}

#modal-content > input {
    margin: 0.7rem;
    font-size: 1rem;
    min-width: 16rem;
    width: fit-content;
    padding: 0.5rem;
    padding-left: 1rem;
    border-radius: 2rem;
    outline: none;
    border: none;
}

#add-sub-title{
    font-size: 1.7rem;
    margin: 1rem;
}

#modal-content {
    width: 50vw;
    height: 50vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    transition: all 0.5s ease;
}

.unblurred{
    transition: all 0.5s ease;
}

.blur {
    filter: blur(7px);
    transition: all 0.5s ease;
}

#add-sub-modal {
    position: fixed;
    /* top: 0;
    left: 0; */
    z-index: 9;
    /* margin-top: 3rem; */
}

#add-sub {
    cursor: pointer;
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
    /* height: 3vh; */
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
    /* height: fit-content; */
    /* margin-top: 8vh; */

    color: white;
    /* border: 1px solid; */
    border-radius: 10px;
    padding: 20px;
    font-size: 1.5rem;
    outline: #ffffff33;
    position: relative;
    /* filter: blur(5px); */
}

#modify-div-main {
    display: flex;
    flex-direction: column;
    /* justify-content: center; */
    align-items: center; 
    color: white;
    position: relative;
    /* margin-top: 5.5rem; */
    /* height: 80vh; */
    /* pointer-events: none; */
}
</style>