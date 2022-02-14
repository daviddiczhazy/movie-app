<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      You can view or edit movies by click on movie title:
      
    </p>
    <h2><button class="add-button" @click="addNewMovie()">Add Movie</button></h2>
    <h3 >List of Movies</h3>
    <hr>

    <EditMovie :data="data" v-if="edit" @close-edit="closeEdit()" @save-edit="saveEdit"></EditMovie>
    
    <AddMovie :id="addId" v-if="add" @close-add="closeAdd()" @save-new-movie="saveNew"></AddMovie>

    <ListOfMovies :Movies="Movies" @edit-movie="editMovie"></ListOfMovies>

  </div>
</template>

<script>
import axios from 'axios'
import EditMovie from '@/components/EditMovie.vue';
import AddMovie from '@/components/AddMovie.vue';
import ListOfMovies from '@/components/ListOfMovies.vue';

export default {
  name: 'Dashboard',
  props: {
    msg: String
  },
  components: {
    EditMovie, AddMovie, ListOfMovies
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

      this.Movies.find( movie => {
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

    saveEdit(values) {

      this.Movies.find(movie => {
        if (movie.id === values.id) {
          movie.name = values.name
          movie.year = values.year
          movie.director = values.director
          movie.runtime = values.runtime
          movie.storyline = values.storyline
        }
      })

      this.edit = false
      this.clearInputs()
    },

    saveNew(values) {
      console.log(" values: " + JSON.stringify(values));
      this.Movies.push(values)
      this.callMoviesDb()
    },

    //General functions...

    closeEdit() {
      this.edit = false
      this.clearInputs()

    },

    closeAdd() {
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

      callMoviesDb() {
       axios.get('http://localhost:3000/Movies').then((response) => this.Movies = response.data )
      
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
