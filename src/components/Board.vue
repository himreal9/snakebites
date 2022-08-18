<template>
  <div class="hello">
    <div class="row board" v-for="item in steps" :key="item">
      <div
        class="col box"
        v-for="step in item"
        :key="step"
        :style="[
          step % 2 == 0
            ? {
                background:
                  'radial-gradient(circle, rgba(238,174,202,1) 0%, rgba(148,187,233,1) 100%)',
              }
            : { background: 'white' },
        ]"
      > <small style="font-weight:300; font-size:40px;" v-if="currPosition==step">🐥</small>
        <small v-if="step == 100" style="padding-top: 20px;">{{ step }} &#127942;</small
        ><small v-else style="padding-top: 20px;">{{ step }}<br v-if="snakehead[step]">{{snakehead[step]}}<br v-if="snaketail[step]">{{snaketail[step]}}</small>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "BoarD",
  props: {
    num: {type:[Number,String]},
    currPosition: Number,
  },
  data() {
    return {
      steps: undefined,
      snakehead:{30:'🐍 head1',58:'🐍 head2',99:'🐍 head3'},
      snaketail:{7:'🐍 tail1',45:'🐍 tail2',9:'🐍 tail3'}
    };
  },
  methods: {
    stepslist() {
      this.steps = [];
      for (let i = 0; i < 100; i++) {
        var k = [
          1 + i,
          2 + i,
          3 + i,
          4 + i,
          5 + i,
          6 + i,
          7 + i,
          8 + i,
          9 + i,
          10 + i,
        ];
        if (i == 10 || i == 30 || i == 50 || i == 70 || i == 90) k.reverse();
        this.steps.push(k);
        i += 9;
      }
      this.steps.reverse();
    },
  },
  mounted() {
    this.stepslist();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url(https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css);

.box {
  border-style: solid;
  border-width: 1px;
  width: 80px;
  height: 60px;
  
}
.board {
  margin-right: auto;
  margin-left: auto;
  width: 1000px;
}
</style>
