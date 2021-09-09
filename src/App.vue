<template>
  <div id="app">
    <RecordNav nav-name="Hello"></RecordNav>

    <main role="main" class="fluid-container m-4">
        <div class='row'>
            <div class="col-8">
              <div v-for="record in localRecords" :key="record.id">
                <RecordCard :record="record"></RecordCard>
              </div>

            </div>
            <div class="col-4">
                <div class="card bg-light mb-3" style="max-width: 18rem;">
                  <div class="card-header">Infos & Filters</div>
                  <span></span>
                  <div class="card-body">
                    <h5 class="card-title">Sort </h5>

                    <select class="custom-select mb-2" @change="handleSort" v-model="selectedSort">
                      <option value="OdrCroi">Notes croissantes</option>
                      <option value="OdrDéc">Notes décroissantes</option>
                    </select>

                    <p class="card-text">Some sorting options</p>
                    <h5 class="card-title">Filter</h5>
                    <a href="#" @click="clickHandler">Test event</a>
                    <br>
                    <button type="button" class="btn btn-dark" @click="clickClassment"></button>
                  </div>
                </div>
            </div>


        </div>


    </main>

  </div>
</template>

<script>
import {records} from "@/assets/static/js/allRecords";
import RecordCard from "@/components/RecordCard";
import RecordNav from "@/components/RecordNav";

export default {
  name: 'App',
  components:{
    RecordCard,
    RecordNav
  },
  data: ()=>({
    localRecords:records,
    hasFullRecords:true,
    btnFilterLabel: ["Only best 20", "All records"],
    currentFilterLabel:'',
    selectedSort:''
  }),
  methods:{
    clickHandler (){
      console.log(this.localRecords)
      this.localRecords[0].artist = 'Modif via link'
    },
    clickClassment() {
      if(this.hasFullRecords) {
        this.localRecords = this.localRecords.filter(record => record.pitchforkPos <= 20)
        this.hasFullRecords=false
        this.currentFilterLabel = this.btnFilterLabel[1]
      }else{
        this.localRecords = records
        this.hasFullRecords=true
        this.currentFilterLabel = this.btnFilterLabel[0]
      }
    },
    created(){
      console.log('coucou')
      this.currentFilterLabel = this.btnFilterLabel[0]
    },
    handleSort(){
      console.log(`Option selectionnée : ${this.selectedSort}`)
      this.sortLocalRecords(this.selectedSort)
    },
    sortLocalRecords(sortOption){
      console.log(sortOption)
      if(sortOption=='OdrCroi') {
        this.localRecords = this.localRecords.sort((recA, recB) => {
          return recA.pitchforkPos - recB.pitchforkPos
        })
      }else{
        this.localRecords = this.localRecords.sort((recA, recB) => {
          return recB.pitchforkPos - recA.pitchforkPos
        })
      }
   }
  }
}

</script>

<style lang="scss">
  @import "./assets/navbar-top-fixed.css";
  @import "../node_modules/bootstrap/scss/bootstrap.scss";
</style>
