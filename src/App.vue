<template>
  <v-app>
    <v-content>
      <!-- <block /> -->
      <v-container>
        <div class="form">
          <input type="text" v-model.lazy="searchName" class="search" placeholder="Search by Name" />
          <div class="button" v-on:click="ajax" id="submit">Submit</div>
          <input type="number" v-model.lazy="searchYear" id="year" class="year" placeholder="Year" />
          <!-- <input type="text" id="ganre" class="ganre" placeholder="Ganre" />
          <input type="text" id="raiting" class="rating" placeholder="TMDb ratings, vote_average" />-->
        </div>
        <div class="row">
          <div class="col-4 wrapper" id="wrapper" v-for="item in resultsSearch" :key="item.id">
            <div class="plate">
              <div class="img">
                <img
                  id="img"
                  v-if="item.backdrop_path==null"
                  src="https://bitsofco.de/content/images/2018/12/broken-1.png"
                />
                <img id="img" v-else v-bind:src="urlImg+item.backdrop_path" />
              </div>
              <div class="title">
                {{item.title}} ({{item.release_date}})
                <div id="modal" class="subtitle-2">{{item.overview}}</div>
              </div>
            </div>
          </div>
        </div>
        <!-- 
        <div class="arrnext" id="arrnext"></div>
        <div class="arrprev" id="arrprev"></div>-->
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
// import block from "./components/block";

export default {
  name: "App",

  components: {
    // block
  },
  data() {
    return {
      arrSearch: [],
      resultsSearch: [],
      searchYear: Number,
      searchName: "",
      // objSearch: {},
      startUrl:
        "https://api.themoviedb.org/3/search/movie?api_key=7d66cc99e9b335766f17c1718166276a&language=en-US",
      urlImg: `https://image.tmdb.org/t/p/w500`,
      gueryName: "&query=",
      nameYear: "&primary_release_year=",
      numPage: "&page=1",
      finalUrl: ""
    };
  },
  methods: {
    query: function() {
      let self = this;
      self.search = self.searchName;
      self.searchName = "";
    },
    year: function() {
      let self = this;
      self.year = self.searchYear;
      self.searchYear = Number;
    },
    createFinalUrl: function() {
      this.query();
      this.year();
      this.finalUrl =
        this.startUrl +
        this.gueryName +
        this.search +
        this.nameYear +
        this.year +
        this.numPage.toString();
    },
    ajax: function() {
      this.createFinalUrl();
      let self = this;
      fetch(this.finalUrl)
        .then(response => response.json())
        .then(function(myJson) {
          self.arrSearch = myJson;

          // создаем масив объектов результатов поиска
          self.resultsSearch = self.arrSearch.results;

          // for (let i = 0; i < self.resultsSearch.length; i++) {
          //   // экземпляр объекта поиска
          //   self.objSearch = self.resultsSearch[i];
          // }
        });
      (this.finalUrl = ""), (self.resultsSearch = []);
    }
  }
};
</script>

<style scoped>
input {
  border: 2px solid rgba(8, 8, 8, 0.767);
  margin: 20px;
  padding: 7px;
  width: 20%;
  height: 50px;
  border-radius: 5px;
}
.button {
  cursor: pointer;
}
.form {
  display: flex;
  justify-content: center;
  align-items: center;
}
.arrnext {
  width: 42px;
  height: 42px;
  background-color: red;
  display: none;
}

.arrprev {
  width: 42px;
  height: 42px;
  background-color: green;
  display: none;
}

img {
  width: 100%;
  max-width: 100%;
  max-height: 100%;
  border-top-left-radius: 5%;
  border-top-right-radius: 5%;
}
.img {
  height: 208px;
  border-top-left-radius: 15px;
  border-top-right-radius: 15px;
}

.wrapper {
  display: flex;
  flex-direction: column;
}
.plate {
  background-color: rgba(255, 255, 255, 0.37);
  height: 100%;
  border-radius: 15px;
}

.plate:hover {
  box-shadow: 0px 13px 28px -6px rgb(128, 126, 126);
  cursor: pointer;
}
.title {
  text-align: center;
  padding: 15px 0;
}
#app {
  background-image: linear-gradient(
      to right top,
      #2a2c2ead,
      #565b5dad,
      #888f8ead,
      #bfc6c1ad,
      #fdfff7ad
    ),
    url(./assets/bg.png);
  background-position: top;
}
.subtitle-2 {
  padding: 5px;
}
</style>