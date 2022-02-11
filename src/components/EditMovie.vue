<template>
    <div class="movie-section" >
      <h3>You can edit your movie</h3>
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

      <p class="paragraph-edit-movie">Movie actors: <span v-for="actor in movieActors" :key="actor.id">
        {{ actor }}, 
      </span> </p>
      <p class="paragraph-edit-movie">Movie writers: <i v-for="writer in movieWriters" :key="writer.id">
        {{ writer }}, 
      </i> </p>
      <p class="paragraph-edit-movie"> <u>Additional informations:</u>  <strong>Categories:</strong>  <span v-for="category in movieCategories" :key="category.id">
        {{ category }}, 
      </span>
          <strong>Release date:  </strong> {{movieRelease}}
      </p>

      <p><a href="#" @click.prevent="saveChanges()">save</a> - <a href="#" @click.prevent="closeEdit()">close</a></p>



    </div>
</template>

<script>
import axios from 'axios'
    export default {
        name: 'EditMovie',
        props: {
            data: Object
        },
        data() {
            return {
                movieId: this.data.movieId,
                movieName: this.data.movieName,
                movieYear: this.data.movieYear,
                movieDirector: this.data.movieDirector,
                movieRuntime: this.data.movieRuntime,
                movieCategories: this.data.movieCategories,
                movieActors: this.data.movieActors,
                movieWriters: this.data.movieWriters,
                movieDescription: this.data.movieDescription,
                movieRelease: this.data.movieRelease,
            }
        },
        methods: {
            /*
            * Function for editing movie via axios put method according movie id.
            */
            saveChanges() {
                axios.put(`http://localhost:3000/Movies/${this.movieId}`,
                {   
                     "id": this.movieId,
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

                this.closeEdit()

            },

            closeEdit() {
                this.$emit("close-edit")
            }
        },
       
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