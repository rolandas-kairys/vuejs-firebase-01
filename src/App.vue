<template>
  <div id="app" class="container">
   <div class="page-header">
      <h2>Vue.js 2 & Firebase <small>Sample Application</small></h2>
        </div>
    <div class="row">
      <div class="col-sm-offset-2 col-sm-7">
    <!-- Add Recipe Form Button -->
         <button class="btn btn-success btn-block" data-toggle="modal" data-target="#formModal" >Add Recipe <span class="glyphicon glyphicon-pencil" aria-hidden="true"></span>  </button><hr />
    <!-- render the list or recipies -->
          <div class="well" v-for="recipe in recipes">
            <h4>{{ recipe.name }} <button class="btn btn-default pull-right" data-toggle="collapse" v-bind:href="'#'+recipe.id" aria-expanded="false" v-bind:aria-controls="recipe.id"><span class="glyphicon glyphicon-cutlery" aria-hidden="true"></span></button></h4>
               <div class="collapse" v-bind:id="recipe.id">
                  <br /><br />
                    <p><strong>Ingredients:</strong> {{ recipe.ingredients }} </p>
                      <p><strong>Directions:</strong> {{ recipe.directions }} </p>
                        <br /><hr />
                          <button class="btn btn-default edit-recipe">Edit <span class="glyphicon glyphicon-edit" aria-hidden="true"></span></button>
                            <button class="btn btn-danger" v-on:click="removeRecipe(recipe)" >Delete <span class="glyphicon glyphicon-trash" aria-hidden="true"></span></button>
                              </div><!-- .collapse -->
                                </div><!-- well -->                  

      
    <!-- Add Recipe Form  Modal -->
<div class="modal fade" id="formModal" tabindex="-1" role="dialog" aria-labelledby="formModalLabel">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <form method="POST" v-on:submit.prevent="addRecipe">
        <div class="modal-header">
          <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
           <h4 class="modal-title" id="myModalLabel">Add Recipe</h4>
             </div>
      <div class="modal-body">
        <div class="form-group">
            <label>Recipe Id:</label>
              <input type="text" id="recipeId"  class="form-control" v-model="newRecipe.id" />
            <label>Recipe Name:</label>
              <input type="text" id="recipeName"  class="form-control" v-model="newRecipe.name" />
                <label>Ingredients:</label>
                  <textarea rows="6" id="ingredients"  class="form-control" v-model="newRecipe.ingredients" ></textarea>
                    <label>Directions</label>
                      <textarea rows="6" id="directions"  class="form-control" v-model="newRecipe.directions"></textarea>
                        </div>
                          </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
          <input type="submit" class="btn btn-primary" value="Save Changes" />
            </div>
      </form>
    </div>
  </div>
</div>
    </div><!-- col-sm-7 -->
  </div><!-- row -->
</div><!-- container -->
</template>





<script>
import Hello from './components/Hello'
import Firebase from 'firebase'
import toastr from 'toastr'
let config = {
  apiKey: 'AIzaSyDXzk48nZXCQ4ciIJvzk-Rg9B33KIF-dVc',
  authDomain: 'contacts-d0aab.firebaseapp.com',
  databaseURL: 'https://contacts-d0aab.firebaseio.com',
  projectId: 'contacts-d0aab',
  storageBucket: 'contacts-d0aab.appspot.com',
  messagingSenderId: '392078188679'
}

let app = Firebase.initializeApp(config)
let db = app.database()
let recipesRef = db.ref('recipes')

export default {
  name: 'app',
  firebase: {
    recipes: recipesRef
  },

  data () {
    return {
      newRecipe: {
        name: '',
        ingredients: '',
        directions: '',
        id: ''
      }
    }
  },

  methods: {
    addRecipe: function () {
      recipesRef.push(this.newRecipe)
      this.newRecipe.name = ''
      this.newRecipe.ingredients = ''
      this.newRecipe.directions = ''
      this.newRecipe.id = ''
    },
    removeRecipe: function (recipe) {
      recipesRef.child(recipe['.key']).remove()
      toastr.success('Recipe removed successfully')
    }
  },

  components: {
    Hello
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
