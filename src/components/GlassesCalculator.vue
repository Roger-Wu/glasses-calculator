<script>
export default {
  name: 'GlassesCalculator',
  data() {
    return {
      input_myopia_degree: '',
      default_myopia_degree: '0',
      input_distance_1: '',
      input_distance_2: '',
      default_distance_1: '1',
      default_distance_2: 'inf',
      degree: 100,
      degree_str: '+100',
      is_input_myopia_incorrect: false,
      myopia_error: '近視度數請輸入 0 或正整數',
      is_input_distances_incorrect: false,
      distance_error: '距離請輸入正數或 inf (代表無窮遠)',
    }
  },
  watch: {
    input_myopia_degree() { // new_value, old_value
      this.calc_degree();
    },
    input_distance_1() { // new_value, old_value
      this.calc_degree();
    },
    input_distance_2() {
      this.calc_degree();
    }
  },
  methods: {
    calc_degree() {
      // validate input myopia degree
      let myopia_degree = this.parse_myopia_degree(this.input_myopia_degree, this.default_myopia_degree);
      this.is_input_myopia_incorrect = isNaN(myopia_degree);

      // validate input distances
      let diopter_1 = this.distance_to_diopter(this.input_distance_1, this.default_distance_1);
      let diopter_2 = this.distance_to_diopter(this.input_distance_2, this.default_distance_2);
      this.is_input_distances_incorrect = isNaN(diopter_1) || isNaN(diopter_2);

      if (this.is_input_myopia_incorrect || this.is_input_distances_incorrect) {
        this.degree = 0;
        this.degree_str = '';
        return;
      }
      
      this.degree = -myopia_degree + (diopter_1 - diopter_2) * 100;
      this.degree_str = this.format_degree(this.degree);
    },
    format_degree(degree_num) {
      return (degree_num > 0? "+" : "") + Math.round(degree_num).toString();
    },
    parse_myopia_degree(input_myopia_degree, default_myopia_degree) {
      if (input_myopia_degree == '') {
        return this.parse_myopia_degree(default_myopia_degree, '0');
      }
      let is_integer = /^(0|[1-9]\d*)$/.test(input_myopia_degree);
      if (!is_integer) {
        return NaN;
      }
      let myopia_degree = parseInt(input_myopia_degree);
      if (isNaN(myopia_degree) || myopia_degree < 0) {
        return NaN;
      }
      return myopia_degree;
    },
    distance_to_diopter(distance_str, default_distance) {
      if (distance_str == '') {
        return this.distance_to_diopter(default_distance, '1');
      }
      if (distance_str == 'inf') {
        return 0;
      }
      let distance_num = Number(distance_str);
      if (isNaN(distance_num) || distance_num < 0) {
        return NaN;
      }
      return 1 / distance_num;
    },
  }
}
</script>

<template>
  <div id="glasses_calculator">
    <p>
      我有 <input class="input_myopia_degree" v-model="input_myopia_degree" :placeholder="default_myopia_degree" /> 度的近視<br>
      我想讓距離眼睛 <input class="input_distance" v-model="input_distance_1" :placeholder="default_distance_1" /> 公尺遠的東西<br>
      看起來像在對焦 <input class="input_distance" v-model="input_distance_2" :placeholder="default_distance_2" /> 公尺遠的東西<br>
    </p>
    <p class="result-wrapper">
      <span v-if="(!is_input_myopia_incorrect) && (!is_input_distances_incorrect)">
        則我需要 {{ degree_str }} 度的<span v-if="degree > 0">老花</span><span v-if="degree < 0">近視</span>眼鏡
      </span>
      <span v-else>
        <span v-if="is_input_myopia_incorrect" class="error_msg">{{ myopia_error }}<br></span>
        <span v-if="is_input_distances_incorrect" class="error_msg">{{ distance_error }}<br></span>
      </span>
    </p>
  </div>
</template>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.input_distance, .input_myopia_degree {
  width: 2rem;
  border: none;
  border-bottom: 1px solid;
  text-align: center;
  border-radius: 0;
  padding: 0 1px;
}
input::placeholder {
  color: #AAA;
}
.error_msg {
  color: red;
}
.result-wrapper {
  height: 20rem;
}
</style>
