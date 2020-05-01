<template>
  <v-app>
    <v-content class="indigo lighten-5 deep-purple--text"> 
      <v-container class="pt-2">
        <v-row class="mx-0 my-0">
          <v-col cols="12" md="5" class="py-0 px-2 pt-1">
            <v-container class="py-0 px-0">
              <v-row class="mx-0">
                <v-col class="px-0 px-2">
                <v-text-field
                  dense
                  class="title deep-purple--text text--accent-4"
                  color="deep-purple accent-3"
                  type="number"
                  label="Bill Amount"
                  v-model.number="check"
                  value="10.00"
                ></v-text-field>
                </v-col>
              </v-row>
              <v-row class="mx-0">
                <v-col class="py-0 pl-0">
                  <div class="mb-1 mt-0">Where you are?</div>
                  <v-btn-toggle
                    v-model="selected"
                    dense
                    group
                  >
                  <v-btn class="py-0" color="white mx-0 px-5 mb-1" v-for="(country,key) in countries"
                  :key="key" :value="country">
                  {{key}}
                  </v-btn>
                  </v-btn-toggle>
                </v-col>
                </v-row>
                <v-row class="mx-0">
                  <v-col class="px-0">
                    <div class="my-0 pb-0">Rate your service</div>
                      <v-list class="py-0 mb-1 mt-0">
                        <v-list-item-group v-model="tips">
                          <v-list-item
                            dense
                            class="deep-purple--text text--accent-4"
                            v-for="(tip, key) in selected"
                            :value="tip"
                            :key="key"
                            >
                            <v-list-item-icon>
                              <v-icon v-text="smiles[key]"></v-icon>
                            </v-list-item-icon>
                            <v-list-item-content>
                              <v-list-item-title v-text="key"></v-list-item-title>
                            </v-list-item-content>
                          </v-list-item>
                        </v-list-item-group>
                      </v-list>
                    </v-col>
              </v-row>
              <v-row class="mx-0">
                <v-col class="pr-3">
                  <div class="mt-1">Custom %</div>
                   <v-slider
                    v-model="tips"
                    :tick-labels="range"
                    thumb-label
                    track-color="blue-grey lighten-4"
                    color="deep-purple"
                    ticks
                    step="1"
                    :thumb-size="24"
                    tick-size="1"
                    min="0"
                    :max="selected.very"
                  >
                  </v-slider>
                </v-col>
              </v-row>
              <v-row class="mx-0">
                <v-col class="text-center px-0" cols="3">
                  <div class="subheading px-0 font-weight-light">Percentage</div>
                    <span
                      class="display-2 font-weight-light"
                      v-text="tips"
                    ></span>
                    <span class="subheading px-0 font-weight-light">%</span>
                </v-col>
                <v-col class="text-center px-0" cols="6">
                  <div v-if="this.check" class="subheading font-weight-light">Total tips</div>
                  <span
                    v-if="this.check"
                    class="display-2 px-0 font-weight-light"
                    v-text="total_tips"
                  ></span>
                  <span v-if="this.check" class="subheading px-0 font-weight-light">{{currency[selected_country]}}</span>
                </v-col>
                <v-col class="text-center px-0" cols="3">
                  <div v-if="this.check" class="subheading font-weight-light">Total</div>
                  <span
                    v-if="this.check"
                  class="display-2 px-0 font-weight-light"
                  v-text="total_check"
                  ></span>
                <span v-if="this.check" class="subheading font-weight-light">{{currency[selected_country]}}</span>
                  </v-col>
              </v-row>
            </v-container>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  components: {
  },

  data(){
    return {
      tips: null,
      total: null,
      check: null,
      smiles: {bad: 'mdi-emoticon-confused-outline', normal: 'mdi-emoticon-neutral-outline', good: 'mdi-emoticon-happy-outline', very: 'mdi-emoticon-excited-outline'},
      emoji: {bad: '😕', normal: '😐', good: '🙂', very: '😄'},
      currency: {USA: '$', Europe: '€', Asia: '¥'},
      countries: {
      USA: {bad: 10, normal: 15, good: 20, very: 25},
      Europe: {bad: 5, normal: 10, good: 12, very: 15},
      Asia: {bad: 2, normal: 5, good: 7, very: 10}
      },
      selected: {},
    }
  },
  watch: {
    selected(value){
      this.tips = value.good;
    }
  },
  mounted(){
    this.selected = this.countries.Europe;
  },
  computed: {
    range(){
      const arr = [];
      const reversedSelected = {};
      for (let [key, value] of Object.entries(this.selected)) {
        reversedSelected[value] = key;
      }
      for (let i = 0; i<=this.selected.very; i++){
        arr.push(this.emoji[reversedSelected[i]] || '');
      }
      return arr;
    },
    selected_country(){
      for (let key in this.countries){
        if (this.countries[key] === this.selected){
          return key;
        }
      }
      return ''; 
    },
    total_tips(){
      let tips = this.check*this.tips/100;
      console.log()
      return tips;
    },
    total_check(){
      return this.total_tips+this.check; 
    }
  }
};
</script>
<style>


</style>