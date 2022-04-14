<template>
  <table id="customers">
    <thead>
      <tr>
        <th>Id</th>
        <th>Nom</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="movie in movies">
        <td>{{ movie.id }}</td>
        <td>{{ movie.title }}</td>
      </tr>
    </tbody>
  </table>
  <div>
    <input @keyup.enter="storeMovie" type="text" v-model="inputMovie.title" placeholder="Ajouter un film">
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  data() {
    return {
      movies : null,
      inputMovie: {
        title : null
      }
    }
  },
  methods : {
    loadMovies() {
      axios.get('http://localhost:3000/films')
        .then(
            (resultat) => {
              this.movies = resultat.data
            }
        )
      .catch(
          (erreur) => {
            console.log(erreur)
          }
      )
    },
    storeMovie() {
      if (this.inputMovie.title != null && this.inputMovie.title.trim() != '') {
        axios.post('http://localhost:3000/films', this.inputMovie)
        .then(
            (resultat) => {
              this.loadMovies()
            }
        )
        .catch(
            (erreur) => {
              console.log(erreur)
            }
        )
      }
    }
  },
  mounted() {
    this.loadMovies()
  },
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#customers {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

#customers td, #customers th {
  border: 1px solid #ddd;
  padding: 8px;
}

#customers tr:nth-child(even){background-color: #f2f2f2;}

#customers tr:hover {background-color: #ddd;}

#customers th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #04AA6D;
  color: white;
}

input[type=text], select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}
</style>
