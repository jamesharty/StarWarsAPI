<template>
  <div class="hello">
    <h1 class="intro">{{ msg }}</h1>
    <p class="intro">
      Below there is a list of characters from the Star Wars universe. <br>
      By using the search bar, you may filter these characters. <br>
      Clicking on each of these individual character names will display more details.
    </p>
      
      <div class="searchContainer">
      <input type="text" v-model="search" placeholder="Search Character Name.."/>
      </div>
      <div class="tableContainer">
        <table class="table table-hover table-dark">
          <thead>
            <tr>
              <th>Name</th>
              <th>Height(cm)</th>
              <th>Mass(kg)</th>
              <th>Created</th>
              <th>Edited</th>
              <th>Planet Name</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="person in filteredPeople" v-bind:key="person.id ">
              <td>{{ person.name }}</td>
              <td>{{ person.height }}</td>
              <td>{{ person.mass }}</td>
              <td>{{ person.created }}</td>
              <td>{{ person.edited }}</td>
              <td>{{ person.homeworld }}</td>
            </tr>
        </tbody>
        </table>
      </div>    
    </div>
</template>

<script>
export default {
  name: 'StarWarsAPI',
  props: {
    msg: String
  },
  data: function () {
    return {
      search: '',
      people: [],
    };
  },
  methods: {

    //Queries the API and gathers the results
    async getAllData() {
      try {

        let next = "https://swapi.dev/api/people"
    
    // API retrieves 10 results per page, so loop continues until all characters are collected
        while(next){
        let response = await fetch(next);
        var responseResult = await response.json();
        next = responseResult.next;
        var listOfPeople = responseResult.results

          //creates individual person objects with the details required and stores them in array for display
         for (var indvidualPerson in listOfPeople){
          //console.log(x.results[indvidualPerson])
          const newPerson = {name: listOfPeople[indvidualPerson].name,
                            height: listOfPeople[indvidualPerson].height,
                            mass: listOfPeople[indvidualPerson].mass,
                            created: listOfPeople[indvidualPerson].created,
                            edited: listOfPeople[indvidualPerson].edited,
                            homeworld: listOfPeople[indvidualPerson].homeworld,
                  }


          //gets the users homeworld by using the API query stored in the user homeworld property when retrieving the data
          newPerson.homeworld = await this.getPlanet(newPerson.homeworld)
          //adds this user object to the array
          this.people.push(newPerson)
          }

        }
        
      } catch (error) {
        console.log(error);
      }
    },
    //method to retrieve person planet
    async getPlanet(queryAPI){
      try {

        //queries the API and retrieves planet object result, returns the planets name
        const response = await fetch(queryAPI)
        const planet = await response.json();
        return planet.name
      } catch (error) {
              console.log(error);
            }
      },
      searchFunction(){
          console.log("test")
      },
  },
  
  created() {
    this.getAllData();
  },
  computed: {
    filteredPeople(){
     // function to compare names
     //this portion of code found on stackoverflow, adapted to use for this project, compares inputted search term to the names of the person objects
     function compare(a, b) {
       if (a.name < b.name) return -1;
       if (a.name > b.name) return 1;
       return 0;
     }
      
     return this.people.filter(user => {
        return user.name.toLowerCase().includes(this.search.toLowerCase())
     }).sort(compare)
    }
  },
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
}
.intro {

   color: #FFE919;
  }

.tableContainer{
    text-align: left;
    margin-left: auto;
    margin-right: auto;
    width: 60%;
  }

.searchContainer{
  padding: 2%;
}

input {
  width: 40%
}


</style>
