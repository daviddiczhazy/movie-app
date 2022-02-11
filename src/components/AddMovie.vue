<template>
    <div class="movie-section" >
      <h3>You can add new movie</h3>
      <h4></h4> 
      <label for="movieName" class="input-edit-movie"> <strong>Movie name: </strong>  
      <input v-model="movieName" type="text" name="movieName" id="movieName" />
      </label>

      

      <label for="movieDirector" class="input-edit-movie" > <strong>Director: </strong>  
      <input v-model="movieDirector" type="text" name="movieDirector" id="movieDirector" />
      </label>

      <label for="movieYear"> <strong>Year: </strong>  
      <input v-model="movieYear" type="text" name="movieYear" id="movieYear" />
      </label>

      <label for="movieRuntime"> <strong>Duration: </strong>  
      <input v-model="movieRuntime" type="text" name="movieRuntime" id="movieRuntime" />
      </label>
      

      <label for="movieDescription" class="input-edit-movie" > <strong>Description: </strong>  
      <textarea v-model="movieDescription"  rows="4" cols="50" name="movieDescription" id="movieDescription" />
      </label>

      <p><a href="#" @click.prevent="saveNewMovie()">add</a> - <a href="#" @click.prevent="closeAdd()">close</a></p>



    </div>
</template>

<script>
import axios from 'axios'
    export default {
        name: 'AddMovie',
        props: {
            id: Number
        },
        data() {
            return {
                movieId: this.id,
                movieName: null,
                movieYear: null,
                movieDirector: null,
                movieRuntime: null,
                movieCategories: null,
                movieActors: null,
                movieWriters: null,
                movieDescription: null,
                movieRelease: null,
            }
        },

        methods: {
            
            /*
            * This function create anew movie and add it into database via axios post request. 
            */
            saveNewMovie() {
            axios.post('http://localhost:3000/Movies', 
                 {   "id": this.movieId,
                     "name": this.movieName,
                     "year": this.movieYear,
                     "runtime": this.movieRuntime,
                     "categories": this.movieCategories,
                     "release-date": "1979-05-25",
                     "director": this.movieDirector,
                     "writer": this.movieWriters,
                     "actors": this.movieActors,
                     "storyline": this.movieDescription
                    }
            )

            // clear the inputs
            this.clearInputs() 
            this.closeAdd()

    },
    closeAdd() {
        this.$emit("close-add")
    },
    clearInputs() {
                this.movieId = "",
                this.movieName = "",
                this.movieYear= "",
                this.movieDirector= "",
                this.movieRuntime= "",
                this.movieCategories= "",
                this.movieActors= "",
                this.movieWriters= "",
                this.movieDescription= ""
    }
  }
}
</script>

<style scoped>
.movie-section {
  width: 50%;
  margin: 0 auto;
  border: 2px dotted #42b983;
}

.input-edit-movie {
  display: block;
  margin: 1em;
}
</style>