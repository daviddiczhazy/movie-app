<template>
    <div v-for="movie in Movies" :key="movie.id" class="movie-box">
        <div class="movie-title-box" @click.prevent="editMovie(movie.id)">
            <h2 class="movie-title"> {{ movie.name }}  </h2>
            <p class="movie-tags"><span v-for="movie in movie.categories" :key="movie.id" class="movie-tag">
                 {{ movie }}
                </span>
            </p>
        </div>
        <p class="movie-director"><strong>{{movie.director}}</strong>, <span> <i>{{movie.year}}</i></span></p>
            <a href="#" :title="movie.actors" >Actors</a> | <a href="#" :title="movie.writer"> Writers</a> | <span>{{movie.runtime}}min.</span> <small>{{movie.id}}</small>
            <hr>
        <p class="movie-description" >{{movie.storyline}}</p>
    </div>
</template>

<script>
    export default {
        name: 'ListOfMovies',
        emits: [ 'edit-movie'],
        props: {
            Movies: Array
        },
        data() {
            return {
                
            }
        },

        methods: {

            editMovie(id) {
                this.$emit("edit-movie", id)
            }
        }
    }
</script>

<style  scoped>

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
</style>