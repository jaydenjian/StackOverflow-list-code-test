<template>
  <div class="navBar">
    <div class="searchBar">
      <input type="text" class="input" placeholder="Tag" v-model="input">
      <button class="searchButton" @click="searchKeyTag()" >Search</button>
    </div>
    <div class="title">
      <div class="trendingTitle">Trending</div>
    </div>
    <div class="tags" id="tags">
      <template v-for="(value, index) in tags" :key="index"> 
        <div class="trendingTag" v-if="index < 10" :class="{'selected':index==0}" @click="checkSelected($event)"> {{value.name}} </div>
      </template>    
    </div>
  </div>
  <Comment v-for="question in questions" :item="question" :key="question" class="comments"/>
  <Observer v-on:intersect ="intersected"/>
  
</template>

<script>
import Observer from './components/Observer.vue'
import Comment from './components/Comment.vue'

export default {
  name: 'app',
  components:{
    Observer,Comment,
  },
  data(){
    return{
      tags: [],
      questions:[],
      page: 1,
      keyTag: "javascript",//questionlist subject
      apiUrl: "",
      input:"",
    }
  },
  mounted(){ 
    fetch('https://api.stackexchange.com/2.3/tags?order=desc&sort=popular&site=stackoverflow')
    // fetch('http://localhost:3000/items')
      .then(console.log("===tag==="))
      .then(res => res.json())
      .then(data => (this.tags = data.items))
      .catch(err=>console.log(err.message))
  
  },
  methods:{
    async intersected(){
      this.generateUrl();
      //fetch to stackoverflow
      const res = await fetch(this.apiUrl);
      const items = await res.json();
      // this.questions = [...new Set([...this.questions,...items.items])];
      this.questions = [...this.questions,...items.items];

      //test on local
      // const res = await fetch('http://localhost:3000/items');
      // const items = await res.json();
      // this.questions = [...new Set([...this.questions,...items])];

      console.log("==question==");
      console.log(this.questions);      
      console.log("Now page:"+this.page);
      this.page++;
    },

    searchKeyTag(){
      console.log("===input===")
      console.log(this.input);

      //Check the input whether in trending tags
      //get the parent div
      var parentTag = document.getElementById('tags');      
      var childTags=parentTag.children;  
      var hasSearchKeyTag = false;
      
      for(var i=0; i<childTags.length ;i++){
        childTags[i].classList.remove("selected"); //clean all tag "selected" class

        if(childTags[i].textContent==this.input){ 
          childTags[i].classList.add("selected"); //giv tag the "selected" class
          this.keyTag=childTags[i].textContent; // set key tag as search key
          hasSearchKeyTag=true;
        }
      }

      if(!hasSearchKeyTag){
        this.keyTag=this.input;        
      }
      // refresh web
      this.page=1;
      this.questions=[];
      

    },

    generateUrl(){
      this.apiUrl= "https://api.stackexchange.com/2.3/questions?page="+this.page+"&pagesize=20&order=desc&sort=activity&tagged="+this.keyTag+"&site=stackoverflow";
      console.log("=== key word ===");
      console.log(this.keyTag);
      console.log("===url===");
      console.log(this.apiUrl);
    },

    checkSelected(event){
      //get the parent div, clean all tag "selected" class
      var parentTag = document.getElementById('tags');      
      var childTags=parentTag.children;  
      for(var i=0; i<childTags.length ;i++){
        childTags[i].classList.remove("selected");
      }
      //let clicked tags add "selected" class
      event.target.classList.add("selected");

      //change Key Tag      
      this.keyTag = event.target.textContent;

      //refresh web
      this.page=1;
      this.questions=[];
    }
  },
}
</script>

<style>
/* *{
  border: solid 1px black;
} */
html, body{
  width: 100%;
  height: 100%;
  border: none;  
  margin: 0px;
  padding: 0px;
  background-color: white;
  font-size: 15px;
  display:flex;
  justify-content: center;
  align-items: flex-start;
}

#app {
  font-family: 'Noto Sans',Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale; 
  display: flex;
  align-content: center;
  justify-content: center;
  flex-wrap: wrap;
  max-width: 800px; 
}
/* *********** Nav (Fixed Part) ********** */
.navBar{
  position: fixed;
  z-index: 2;
  background-color: #fff;
  top: 0;
  padding-top: 50px;
  padding-bottom: 15px;
}
/* ***********Search Bar********** */
.searchBar{
  width: 100%;
  display: flex;
  align-content: center;
  justify-content: center;
}
.input{
  border-radius: 7px 0px 0px 7px;
  border-color: #AED9E6;
  border-width: 2px;
  width: 750px;
  height: 35px;
  padding: 5px 0px 5px 10px;
  box-sizing: border-box;
}
.searchButton{
  border-radius: 0px 7px 7px 0px;
  padding: 5px 20px;
  text-align: center;
  color: black;
  background-color: #AED9E6;
  border-color:#AED9E6;
  cursor: pointer;
}

/* ***********Tags********** */
.title{
  width: 100%;
  display:flex;
  justify-content: center;
  align-content: center;
}
.trendingTitle{
  font-size: 30px;
  width: 100%;
}
.tags{
  display: flex;
  flex-wrap: wrap;
  width: 100%;
}
.trendingTag{
  padding: 5px;
  border-radius: 5px;
  border: solid 1px #AED9E6;
  margin-right: 10px;
  background-color: #fff;
}
.selected{
  background-color: #AED9E6;
}
.trendingTag:hover{
  cursor: pointer;
  background-color: #AED9E6;
}

/* ***********Comments Part********** */
.comments{
  position: absolute;
  top: 170px;
}


</style>
