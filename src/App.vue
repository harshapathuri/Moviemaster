<template>
   <div id="app"  >
      <div class="wrapper d-flex align-items-stretch">
         <nav id="sidebar">
            <div class="custom-menu">
               <button type="button" id="sidebarCollapse" class="btn btn-primary">
               </button>
            </div>
            <div class="img bg-wrap text-center py-4" style="background-image: url(images/logo.png);height:230px">
               <div class="user-logo">
                  <h3> </h3>
               </div>
               <form action="#" class="header__search">
                  <div class="container">
                     <div class="row">
                        <div class="col-12">
                           <div class="header__search-content">
                              <input type="text" placeholder="Search for a movie, TV Series that you are looking for" v-model="search">
                              <button type="button" v-on:click="searchResult()">Search</button>
                           </div>
                        </div>
                     </div>
                  </div>
               </form>
            </div>
            <ul class="list-unstyled components mb-1" v-for="(item, index) in movies">
               <li v-on:click="actOnMenu(item.imdbID)"><span class="fa fa-picture-o mr-3"></span>  {{ item.Title }}  -  {{ item.Year }}</li>
            </ul>
            <div class="row">
            <div class="columnNav">
            <span class="fa fa-chevron-circle-left  mr-3" v-on:click="leftNavigate()"></span>
            </div> 
            <div class="columnNav">
              <div >Pages</div>
            <div >{{totalPages}} results</div>
            </div>
            
            <div class="columnNav">
              <span class="fa fa-chevron-circle-right  mr-3" v-on:click="rightNavigate()"></span>
              </div>
             </div>
         </nav>
         <!-- Page Content  -->
         <div id="content" class="p-4 p-md-5 pt-5" >
            <div class="col-12 col-xl-12" style="background-image: url(images/logo.png);">
               <div class="card card--details">
                  <div class="row">
                     <!-- card cover -->
                     <!-- end card cover -->
                     <!-- card content -->
                     <div class="col-12 col-sm-8 col-md-8 col-lg-9 col-xl-7">
                        <h1>Selected Movie Title</h1>
                      <span>  {{Genre}}</span>
                        <div class="card__content">
                           <div class="card__wrap">
                           </div>
                           <div class="card__description card__description--details">
                              Movie Plot - {{Plot}}
                           </div>
                          
                           
                          <span> <h3>Language:</h3>
                           {{Language}}</span> 
                           <h3><span>Director:</span> </h3>
                           {{Director}}
                           <h3><span>Actor:</span> </h3>
                           {{Actor}}
                           <h3><span>Duration:</span> </h3>
                           {{Duration}}
                        </div>
                     </div>
                     <div class="col-12 col-sm-4 col-md-4 col-lg-3 col-xl-5">
                        <div class="card__cover">
                           <img v-bind:src='Image' alt="">
                        </div>
                     </div>
                     <!-- end card content -->
                  </div>
               </div>
            </div>
         </div>
      </div>
   </div>
</template>
<script>
   import axios from 'axios';
   
   export default {
     name: "get-request-error-handling",
     data() {
       return {
         movies: null,
         errorMessage: null,
         Language :null,
         Director: null,
         Actor:null,
         Duration:null,
         Plot:null,
         Genre:null,
         Image:null,
         search:null,
         totalPages:0,
         PageNumber:1,
       };
     },
     methods: {
       actOnMenu: function (imdbId) {
       axios.get("http://www.omdbapi.com/?apikey=127424fc&i="+imdbId)
         .then(response => {
         this.Language = (response.data.Language);
         this.Director = (response.data.Director);
         this.Actor = (response.data.Actors);
         this.Duration = (response.data.Runtime);
         this.Plot = (response.data.Plot);
         this.Genre = (response.data.Genre);
         this.Image = response.data.Poster
   
         })
         .catch(error => {
           this.errorMessage = error.message;
         });
       },
       searchResult: function(){
        
      axios.get("http://www.omdbapi.com/?apikey=127424fc&s="+this.search+"&page=1") 
         .then(response => {this.movies = (response.data.Search);
         this.totalPages =response.data.totalResults
         })
         .catch(error => {
           this.errorMessage = error.message;
         });
       },
       leftNavigate : function(){
         this.PageNumber =this.PageNumber-1
        axios.get("http://www.omdbapi.com/?apikey=127424fc&s="+this.search+"&page="+this.PageNumber) 
         .then(response => {this.movies = (response.data.Search);
         this.totalPages =response.data.totalResults
         })
         .catch(error => {
           this.errorMessage = error.message;
         });
       },
       rightNavigate: function(){
           this.PageNumber =this.PageNumber+1
      axios.get("http://www.omdbapi.com/?apikey=127424fc&s="+this.search+"&page="+this.PageNumber) 
         .then(response => {this.movies = (response.data.Search);
         this.totalPages =response.data.totalResults
         })
         .catch(error => {
           this.errorMessage = error.message;
         });
       }
     },
     created() {
       // GET request using axios with error handling
       axios.get("http://www.omdbapi.com/?apikey=127424fc&s=&page=1")
         .then(response => {this.movies = (response.data.Search);
        
         })
         .catch(error => {
           this.errorMessage = error.message;
         });
     }
   };
</script>