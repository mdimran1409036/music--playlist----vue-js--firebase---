<template>

    <div class="add-song">

        <button v-if = "!showForm" @click="showForm=true">Add Song</button>
        <form action="" v-if="showForm" @submit.prevent="handleSubmit">
            <h4>Add a new Song</h4>

            <input type="text" placeholder="Song title" v-model="title" required>
            <input type="text" placeholder="Artist" v-model="artist" required>
            <button type="submit">Add</button>
        
        </form>
    </div>
</template>

<script>
import { ref } from 'vue'
import useDocument from '@/composables/useDocument'
export default {

props:['playlist'], 

setup(props){

    const title = ref('')
    const artist = ref('')
    const showForm = ref(false) 
    const {updateDoc} =useDocument('Playlists', props.playlist.id)
    
    const handleSubmit =  async () => {
        const newSong= {

            title: title.value,
            artist: artist.value,
            id: '_' + Math.floor(Math.random() * 1000000) + Math.random() .toString(36).substr(2, 9)
        } 
        await updateDoc({

            songs: [...props.playlist.songs, newSong]  
        })

        title.value = ''
        artist.value = ''

        
        
    }

    return {title, artist, showForm, handleSubmit}
}



}
</script>

<style scoped>

    .add-song{
        text-align: center;
        margin-top: 40px;
    }
    form{

        max-width: 100%;
        text-align: left;
    }
</style>