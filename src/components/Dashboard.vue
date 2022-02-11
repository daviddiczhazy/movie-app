<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      You can view or edit movies by click on movie title:
      
    </p>
    <h2><button class="add-button" @click="addNewMovie()">Add Movie</button></h2>
    <h3 >List of Movies</h3>
    <hr>

    <EditMovie :data="data" v-if="edit" @close-edit="closeEdit()"></EditMovie>
    
    <AddMovie :id="addId" v-if="add" @close-add="closeAdd()" ></AddMovie>

    
 
  <div v-for="movie in Movies" :key="movie.id" class="movie-box" >
    <div class="movie-title-box" @click.prevent="editMovie(movie.id)">
      <h2 class="movie-title"> {{ movie.name }}  </h2>
      <p class="movie-tags"><span v-for="movie in movie.categories" :key="movie.id" class="movie-tag">
        {{ movie }}
      </span></p>
    </div>

    <p class="movie-director"><strong>{{movie.director}}</strong>, <span> <i>{{movie.year}}</i></span></p>
    <a href="#" :title="movie.actors" >Actors</a> | <a href="#" :title="movie.writer"> Writers</a> | <span>{{movie.runtime}}min.</span> <small>{{movie.id}}</small>
    
    <hr>
    <p class="movie-description">{{movie.storyline}}</p>
    
  </div>


  </div>
</template>

<script>
import axios from 'axios'
import EditMovie from '@/components/EditMovie.vue';
import AddMovie from '@/components/AddMovie.vue';

export default {
  name: 'Dashboard',
  props: {
    msg: String
  },
  components: {
    EditMovie, AddMovie
  },
  data() {
    return {
      add: false,
      edit: false,
      addId: null,
      data: {},
      Movies: []
    }
  },

  // Axios get request to the mock database which upload data to Movies array.
  async mounted() {
    try {
      const res = await axios.get('http://localhost:3000/Movies')
      this.Movies = res.data
      }
      catch(e) {
        console.error(e)
      }
  },


  methods: {

    /*
    * function for editing movie on which user clicked
    *
    * @Param: id - Number, id value of exact element.
    * */
    editMovie(id) {

      this.Movies.forEach( movie => {
        if (movie.id === id) {
          this.data.movieId = movie.id,
          this.data.movieName = movie.name
          this.data.movieYear = movie.year
          this.data.movieDirector = movie.director
          this.data.movieRuntime = movie.runtime
          this.data.movieCategories = movie.categories
          this.data.movieActors = movie.actors
          this.data.movieWriters = movie.writer
          this.data.movieDescription = movie.storyline
          this.data.movieRelease = movie["release-date"]
        }
      })
      this.edit = true
    },

    //General functions...

    closeEdit() {
      this.callMoviesDb()
      this.edit = false
      this.clearInputs()

    },

    closeAdd() {
      this.callMoviesDb()
      this.add = false
      this.clearInputs()

    },

     addNewMovie() {
      this.addId = this.Movies.length +1
      this.add = true
      
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
    },

     async callMoviesDb() {
      await axios.get('http://localhost:3000/Movies').then((response) => this.Movies = response.data )
      window.location.reload()
    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
  text-decoration: none;
}

.movie-box {
  position: relative;
  width: 15em;
  height: 24em;
  display: inline-block;
  margin: 1em;
  border: 2px solid grey;
  border-radius: .5em;

}

.movie-title-box {
  position: relative;
  background: grey;
  color: aliceblue;
  height: 9em;
  cursor: pointer;
}

.movie-title {
  position: relative;
  top: 2em;
  
  margin: 0;
  max-height: 6em;
  overflow: hidden;
}

.movie-tags {
  position: absolute;
  top: -0.5em;
  
}

.movie-tag {
  padding: .25em;
  background: #42b983;
  border-radius: .25em;
  color:aliceblue;
  margin: 0 .15em;
}

.movie-description {
  overflow: hidden;
  height: 8em;
  text-align: left;
  padding: 0 .25em;
}


.add-button {
  padding: .5em 1em;
  background: #42b983;
  color: aliceblue;
  font-weight: 700;
  font-size: 1em;
  border-radius: .5em;
  cursor: pointer;
  margin-top: 1em;
}

.add-button:hover {
  color: #eee;
}


</style>
