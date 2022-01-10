<template>
  <div id="app">

    <div class="container">
      <div class="row">
        <div class="col-sm-2"></div>
        <div class="col-sm-8">COVID-19 is primarily airborne. Aerosols that float and slowly fill
          an indoor area are the way you are most likely to catch it.  Masks are your best personal
          defense here. In 2022, You need BETTER MASKS than simple cloth masks.</div>
        <div class="col-sm-10">This table is about improvement in relative protection as you opt for
          better mask combinations</div>
          <div class="col-sm-2"></div>
          <div class="col-sm-10">Rows - masks the infecitous person is wearing</div>
          <div class="col-sm-2"></div>
          <div class="col-sm-10">Columns - masks the person who could be infected is wearing</div>
      </div>
      <br/>
      <div class="row">
        <div class="col-sm-1"></div>
        <div class="col-sm-2 form-check form-switch">
          <input
            v-model="checkWild"
            class="form-check-input"
            type="checkbox"
            id="wild-button"
            checked>
          <label
            class="form-check-label wild"
            for="wild-button"
          >
            Wild
          </label>
        </div>
        <div class="col-sm-2 form-check form-switch">
          <input
            v-model="checkDelta"
            class="form-check-input"
            type="checkbox"
            id="delta-button"
            checked>
          <label
            class="form-check-label delta"
            for="delta-button"
          >
            Delta
          </label>
        </div>
        <div class="col-sm-2 form-check form-switch">
          <input
            v-model="checkOmicron"
            class="form-check-input"
            type="checkbox"
            id="omicron-button"
            checked>
          <label
            class="form-check-label omicron"
            for="omicron-button"
          >
            Omicron
          </label>
        </div>
        <div class="col-sm-3 well form-horizontal">
        <div class="col-sm-2"></div>
          <div class="form-group" style="white-space:nowrap;">
            <div>
              <div class="btn-group" style="display:inline-flex;">
                <div>Figures:</div>
                &nbsp;
                <input type="radio" id="time" value="Time" v-model="xOrTime">
                &nbsp;
                <label for="time">Time</label>
                &nbsp;
                <input type="radio" id="x" value="X" v-model="xOrTime">
                &nbsp;
                <label for="x">Versus Nothing/Nothing - <strong>
                  <span style="background-color: #fefe9a">1.0X</span></strong></label>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <table class="table">
      <thead>
      <tr>
        <th></th>
        <th v-for="header in headers" :key="header">
          {{ header }}
        </th>
      </tr>
      </thead>
      <tbody v-for="(header, index) in headers" :key="header">
        <th scope="row" rowspan="4">{{ header }}</th>
        <tr>
          <td
            v-for="(head, headIndex) in headers"
            :key="headIndex"
            :class="`table-${index}-${headIndex}`"
          >
            <table>
              <tr v-if="xOrTime === 'X' && (checkWild || jsonData[index].wild.x[head] === '1.0X')"
                  class="wild">{{ jsonData[index].wild.x[head] }}</tr>
              <tr v-if="xOrTime === 'X' && checkDelta" class="delta">
                {{ jsonData[index].delta.x[head] }}</tr>
              <tr v-if="xOrTime === 'X' && checkOmicron" class="omicron">
                {{ jsonData[index].omicron.x[head] }}</tr>
              <tr v-if="xOrTime === 'Time' && checkWild"
                  class="wild">{{ jsonData[index].wild.time[head] }}</tr>
              <tr v-if="xOrTime === 'Time' && checkDelta" class="delta">
                {{ jsonData[index].delta.time[head] }}</tr>
              <tr v-if="xOrTime === 'Time'&& checkOmicron" class="omicron">
                {{ jsonData[index].omicron.time[head] }}</tr>
            </table>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import data from '../data.json';
import 'bootstrap/dist/css/bootstrap.min.css';

export default {
  name: 'App',
  data() {
    return {
      headers: [
        'Nothing',
        'Cloth',
        'SM',
        'SM,fit',
        'N95',
        'N95,fit',
      ],
      checkWild: true,
      checkDelta: true,
      checkOmicron: true,
      xOrTime: 'X',
      jsonData: [],
    };
  },
  created() {
    this.jsonData = data;
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  box-sizing: border-box;
}
.form-check .form-check-input {
  float: none;
}
input[type=checkbox] {
  margin-right: 10px;
}
.form-check-input:checked {
  border-color: transparent;
}
#wild-button.form-check-input:checked {
  background-color: #000;
}
#wild-button.form-check-input:focus {
  box-shadow: 0 0 0 0.25rem rgb(48 50 54 / 30%);
}
#delta-button.form-check-input:checked {
  background-color: #f00;
}
#delta-button.form-check-input:focus {
  box-shadow: 0 0 0 0.25rem rgb(54 48 51 / 30%);
}
.wild,
.delta,
.omicron {
  font-weight: bold;
}
.wild {
  color: #000;
}
.delta {
  color: #f00;
}
.omicron {
  color: #00f;
}
table {
  margin: auto;
  /*text-align: center;*/
}
.table>:not(caption)>*>* {
  padding: 10px;
  border-bottom: 0;
  border-right: 1px solid white;
}
.table>:not(:first-child) {
  border: 1px solid white;
}
tbody:first-of-type tr td:first-child {
  background-color: #fefe9a;
}
th {
  font-size: 20px;
}
td.table-0-1,
td.table-0-2,
td.table-1-0,
td.table-1-1,
td.table-1-2,
td.table-2-0,
td.table-2-1,
td.table-2-2 {
  background-color: #f4f269;
}
td.table-0-3,
td.table-0-4,
td.table-1-3,
td.table-1-4,
td.table-2-3,
td.table-2-4,
td.table-3-0,
td.table-3-1,
td.table-3-2,
td.table-3-3,
td.table-4-0,
td.table-4-1,
td.table-4-2 {
  background-color: #dae85d;
}
td.table-4-3,
td.table-3-4 {
  background-color: #bfde52;
}
td.table-4-4 {
  background-color: #a7d347;
}
td.table-0-5,
td.table-1-5,
td.table-2-5,
td.table-3-5,
td.table-4-5,
td.table-5-0,
td.table-5-1,
td.table-5-2,
td.table-5-3,
td.table-5-4 {
  background-color: #41ad1a;
}
td.table-5-5 {
  background-color: #03665c;
}
td.table-5-5 tr:nth-child(1) {
  color: #fff;
}
td.table-5-5 tr:nth-child(2) {
  color: #da85dd;
}
td.table-5-5 tr:nth-child(3) {
  color: #10d3c1;
}
</style>
