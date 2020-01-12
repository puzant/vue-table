<template lang="pug">
  .creators-table-container(v-if="shouldDisplayPopup")
    .table-header
      .number-of-creators
        | ALL Creators ({{numberOfCreators}} Creators)
      .sort-and-filter-container
        .sort-creators( @click="sortCreatorDataByName()" )
          img.sort-logo( src="@/assets/sort.svg" )
          .sort-title Sort
        .filter-creators
          img.filter-logo( src="@/assets/filter.svg" )
          .filter-title Filter

    .table-elements-title
      .element-title.creator-cell Creators Details
      .element-title.impressions-cell Impressions
      .element-title.engagement-cell Engagement
      .element-title.video-views-cell  Video Views
      .element-title.cost-cell  Cost
      .element-title.cpv-cell  CPV
      .element-title.cpe-cell  CPE
    
    .table-data-container

      .group-row(v-for="(creator, index) in creatorsData")
    
        .creator-details-container.creator-cell
          img(:src=' "@/assets/talent-" + creator.id + ".svg"')
          .talent-info-posts
            .talent-name 
              | {{creator.creators_details.talent_name}}
            .talent-posts 
              | {{creator.creators_details.number_of_posts}} posts
          
        .impressions-cell 
          | {{creator.impressions ? creator.impressions : "--"}}
        .engagement-cell.values-container
          .value 
            | {{creator.engagement.value ? creator.engagement.value : "--"}}
          .percentage 
            | {{creator.engagement.percentage ? creator.engagement.percentage + "%" : "--"}}
        .video-views-cell.values-container
          .value 
            | {{creator.video_views.number_of_views ? creator.video_views.number_of_views : "--"}}
          .percentage 
            | {{creator.video_views.view_percentage ? creator.video_views.view_percentage : "--"}}
        .cost-cell 
          | {{creator.cost}}$
        .cpv-cell 
          | {{creator.cpv}}$
        .cpe-cell 
          | {{creator.cpe}}$

    .table-button-container
      .close-popup-button( @click="closePopup()" ) CLOSE
      .view-details-popup-button VIEW DETAILS



</template>

<script lang="ts">
import tableData from '../table_data.js'
import { Component, Prop, Vue } from 'vue-property-decorator';
import * as _ from 'lodash'

@Component
export default class CreatorTable extends Vue {
  creatorsData = tableData.data
  numberOfCreators = 0
  shouldDisplayPopup = true

  created() {
    //  get the number of creators from the creatorsData array of objects
    for(let prop in this.creatorsData) {
      if(this.creatorsData.hasOwnProperty(prop)) 
        this.numberOfCreators++
    }
    return this.numberOfCreators
  }

  closePopup() {
    this.shouldDisplayPopup = false
  }

  sortCreatorDataByName() {
    return this.creatorsData = _.sortBy(this.creatorsData, o => o.creators_details.talent_name)    
  }

}
</script>

<style lang="sass">

.creators-table-container
  box-shadow: 0px 0px 48px -18px rgba(0,0,0,0.75)
  margin: 50px auto
  border-radius: 5px
  height: auto
  width: 991px
  background-color: #fff
  padding: 20px
  color: #111

.table-header
  display: flex
  flex-direction: row
  justify-content: space-between

.number-of-creators
  color: #212121
  font-size: 18px
  font-weight: bold

.sort-and-filter-container
  display: flex
  width: 133px
  justify-content: space-between

.sort-creators,
.filter-creators
  display: flex
  flex-diretion: row
  align-items: center
  &:hover
    cursor: pointer

.filter-logo,
.sort-logo
  margin-right: 10px

.table-elements-title
  display: flex
  width: 991px
  color: #9FA2B4
  margin-top: 32px
  text-align: left

.element-title
  font-size: 14px
  font-weight: 500

.creator-cell
  flex: 2 1 0

.impressions-cell,
.engagement-cell,
.video-views-cell
  flex: 1 1 0

.cost-cell,
.cpe-cell,
.cpv-cell
  flex: 1 1 0

.table-data-container
  width: 991px
  text-align: left
  margin-top: 20px

.creator-details-container
  display: flex
  flex-direction: row

.talent-info-posts
  margin-left: 14px
  text-align: left

.talent-name
  color: #252733
  font-size: 14px
  font-weight: 500

.talent-posts
  color: #116697
  font-size: 11px

.percentage
  color: #116697
  font-size: 11px

.group-row
  display: flex
  align-items: stretch
  margin: 25px 0 0 0

.table-button-container
  display: flex
  justify-content: flex-end

.close-popup-button,
.view-details-popup-button
  margin-left: 10px
  margin-top: 18px
  color: #2196F3
  font-size: 14px
  font-weight: bold
  padding: 5px 15px
  transition: .5s
  &:hover
    cursor: pointer
    background-color: rgba(29, 170, 252, 0.1)
</style>