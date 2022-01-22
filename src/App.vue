<template>

  <div id="app" :dir="direction" style="margin-top: 2px;">

    <div class="grid">
      <div class="row" :dir="direction">
       <span>
         <span v-on:click="doLocale('en', true)">English</span>&nbsp;&nbsp;
         <span v-on:click="doLocale('es', true)">Español</span>&nbsp;&nbsp;
         <span v-on:click="doLocale('pt', true)">Português</span>&nbsp;&nbsp;
         <span v-on:click="doLocale('ar', true)"> العربية</span>&nbsp;&nbsp;
         <span v-on:click="doLocale('th', true)">ไทย</span>&nbsp;&nbsp;
        </span>
      </div>
    </div>
    <br/>

    <div class="container">
      <div class="row">
        <div class="col-sm-2"></div>
        <p class="col-sm-8">{{ $t('covid-message') }} {{ $t('table-info') }}</p>
        <div class="col-sm-2"></div>
      </div>
    </div>
    <button class="btn btn-outline-secondary" v-if="!optionz" v-on:click="optionz = true">Change variants, risk representation or respirator standard</button>
    <div class="container border" v-if="optionz" style="position: relative">
      <button v-on:click="optionz = false" style="position: absolute; border-radius: 15px; top: -10px; right: -10px;">
            X
        </button>
      <div class="row">
        <div class="col-sm-4">
          <div class="container">
            <div class="row" dir="ltr">
              <div class="form-check form-switch">
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
            </div>
            <div class="row" dir="ltr">
              <div class="form-check form-switch">
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
            </div>
            <div class="row" dir="ltr">
              <div class="form-check form-switch">
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
            </div>
          </div>
        </div>
        <div class="col-sm-4">
          <div class="container">
            <div class="well" :dir="direction">
              <div class="form-group">
                <div><strong>{{ $t('Figures') }}</strong></div>
                <input type="radio" id="time" value="Time" v-model="xOrTime">
                &#8239;
                <label for="time">{{ $t('Time') }}</label>
                <br/>
                <span style="white-space:nowrap;">
                <input type="radio" id="x" value="X" v-model="xOrTime">
                &#8239;
                <label for="x">{{ $t('Versus Nothing / Nothing') }} - <strong dir="ltr">
                  <span style="background-color: #fefe9a">1.0X</span></strong></label>
                </span>
              </div>
            </div>
          </div>
        </div>
        <div class="col-sm-4">
          <div class="container">
            <div :dir="direction">
              <div class="form-group">
                <div>
                  <div>
                    <div class="row">
                      <div style="white-space:nowrap;"><strong>{{
                          $t('respirator-standard')
                        }}</strong>
                      </div>
                    </div>
                    <div class="row">
                    <span style="white-space:nowrap;">
                      <input type="radio" id="n95" value="N95" v-model="std"
                             v-on:click="headers = ['Nothing', 'Cloth', 'SM', 'SM,fit', 'N95', 'N95,fit']">
                      &#8239;
                      <label for="n95">🇺🇸 N95</label>
                      &nbsp;&nbsp;
                      <input type="radio" id="ffp2" value="FFP2" v-model="std"
                             v-on:click="headers = ['Nothing', 'Cloth', 'SM', 'SM,fit', 'FFP2', 'FFP2,fit']">
                      &#8239;
                      <label for="ffp2">🇪🇺 FFP2</label>
                    </span>
                    </div>
                    <div class="row">
                    <span style="white-space:nowrap;">
                      <input type="radio" id="kn95" value="KN95" v-model="std"
                             v-on:click="headers = ['Nothing', 'Cloth', 'SM', 'SM,fit', 'KN95', 'KN95,fit']">
                      &#8239;
                      <label for="kn95">🇨🇳 KN95</label>
                      &nbsp;&nbsp;
                      <input type="radio" id="kf94" value="KF94" v-model="std"
                             v-on:click="headers = ['Nothing', 'Cloth', 'SM', 'SM,fit', 'KF94', 'KF94,fit']">
                      &#8239;
                      <label for="kf94">🇰🇷 KF94</label>
                    </span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <br/>

    <h1>{{ $t('good-person') }}</h1>
    <div class="wrapper">
      <div>
        <h1>
          {{ $t('infectious-person') }}
        </h1>
      </div>
      <div class="table-responsive-sm" style="padding-left: 7%">
        <table class="table">
          <thead>
          <tr>
            <th></th>
            <th v-for="header in headers" :key="header" @click="doShowMaskModal(header + '_' + location + '.html')">
              <img v-bind:src="'images/' + header + '.jpg'"><br/>
              <span>{{ $t(header) }}</span>
            </th>
          </tr>
          </thead>
          <tbody v-for="(header, index) in headers" :key="header">
          <th scope="row" rowspan="4" style="width:15%">
            <img v-bind:src="'images/' + header + '.jpg'">
            <span>{{ $t(header) }}</span>
          </th>
          <tr>
            <td style="border-color: blue" v-on:click="doShowRiskModal(index, head)"
              v-for="(head, headIndex) in dataHeaders"
              :key="headIndex"
              :style="`background-color: #${jsonData[index].color[head]}`"
            >
              <table>
                <tr
                  v-if="xOrTime === 'X' && (checkWild || jsonData[index].wild.x[head] === '1.0X')"
                  class="wild"
                >
                  {{ $t(jsonData[index].wild.x[head]) }}
                </tr>
                <tr v-if="xOrTime === 'X' && checkDelta" class="delta">
                  {{ $t(jsonData[index].delta.x[head]) }}
                </tr>
                <tr v-if="xOrTime === 'X' && checkOmicron" class="omicron">
                  {{ $t(jsonData[index].omicron.x[head]) }}
                </tr>
                <tr v-if="xOrTime === 'Time' && checkWild"
                    class="wild">
                  {{
                    jsonData[index].wild.time[head].substring(0, jsonData[index].wild.time[head].indexOf(' ') + 1) +
                    $t(jsonData[index].wild.time[head].substring(jsonData[index].wild.time[head].indexOf(' ') + 1))
                  }}
                </tr>
                <tr v-if="xOrTime === 'Time' && checkDelta" class="delta">
                  {{
                    jsonData[index].delta.time[head].substring(0, jsonData[index].delta.time[head].indexOf(' ') + 1) +
                    $t(jsonData[index].delta.time[head].substring(jsonData[index].delta.time[head].indexOf(' ') + 1))
                  }}
                </tr>
                <tr v-if="xOrTime === 'Time'&& checkOmicron" class="omicron">
                  {{
                    jsonData[index].omicron.time[head].substring(0, jsonData[index].omicron.time[head].indexOf(' ') + 1) +
                    $t(jsonData[index].omicron.time[head].substring(jsonData[index].omicron.time[head].indexOf(' ') + 1))
                  }}
                </tr>
              </table>
            </td>
          </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div class="container">
      <div class="row">
        <div class="col-sm-2"></div>
        <p class="col-sm-8">{{ $t('n95-footer') }}</p>
        <div class="col-sm-2"></div>
      </div>
    </div>
    <Modal v-model="showMaskModal" bg-class="rounded" v-bind:title="$t('mask-info')" wrapper-class="modal-wrapper">
      <iframe style="width: 100%" :src="toShowInMaskModal"></iframe>
      <div class="row">
        <div class="col-sm-12">
          <div class="text-center">
            <button class="btn btn-secondary" type="button" @click="showMaskModal = false">Close</button>
          </div>
        </div>
      </div>
    </Modal>
    <Modal v-model="showRiskModal" bg-class="rounded" title="Risk Info" wrapper-class="modal-wrapper">
      <p v-if="xOrTime === 'Time'">Average time before transmission occurs:
        <span v-if="checkWild">Black time (1st) is for the wild/original variant. </span>
        <span v-if="checkDelta">Red time (2nd) is for the Delta variant. </span>
        <span v-if="checkOmicron">Blue time (3rd) is for the Omicron variant. </span>
      </p>
      <p v-if="xOrTime === 'X'">Relatve risk of transmission versus nobody wearing a mask for
        the original (wild) variant of the virus
              <span v-if="checkWild">Black figure (1st) is for the wild/original variant. </span>
        <span v-if="checkDelta">Red figure (2nd) is for the Delta variant. </span>
        <span v-if="checkOmicron">Blue figure (3rd) is for the Omicron variant. </span>
      </p>
      <div class="row">
        <div class="col-sm-12">
          <div class="text-center">
            <button class="btn btn-secondary" type="button" @click="showRiskModal = false">Close</button>
          </div>
        </div>
      </div>
    </Modal>
  </div>
</template>

<script>
import data from '../data.json';
import 'bootstrap/dist/css/bootstrap.min.css';

import VueModal from '@kouts/vue-modal';
import '@kouts/vue-modal/dist/vue-modal.css';


export default {
  name: 'App',
  data() {
    return {
      dataHeaders: ['Nothing', 'Cloth', 'SM', 'SM,fit', 'N95', 'N95,fit'],
      headers: ['Nothing', 'Cloth', 'SM', 'SM,fit', 'N95', 'N95,fit'],
      checkWild: true,
      checkDelta: true,
      checkOmicron: true,
      xOrTime: 'X',
      std: 'N95',
      jsonData: [],
      location: '',
      windowWidth: 0,
      showMaskModal: false,
      showRiskModal: false,
      optionz: false,
      toShowInMaskModal: '',
      showRiskModalIndex: -1,
      showRiskModalHead: '',
    };
  },
  created() {
    this.jsonData = data;
    this.il8n = this.$i18n;
    let start = window.location.href.lastIndexOf('#') + 1;
    if (start === 0) this.doLocale('', false);
    else this.doLocale(window.location.href.substring(start));
  },
  computed: {
    direction() {
      return this.location === 'ar' ? 'rtl' : 'ltr';
    },
  },
  mounted() {
    const topValue2 = (document.querySelector('.wrapper').offsetHeight - document.querySelector('.wrapper h1').offsetHeight) / 2;
    document.querySelector('.wrapper h1').style.top = `${topValue2}px`;
    window.addEventListener('resize', () => {
      this.windowWidth = window.innerWidth;
      const topValue = (document.querySelector('.wrapper').offsetHeight - document.querySelector('.wrapper h1').offsetHeight) / 2;
      document.querySelector('.wrapper h1').style.top = `${topValue}px`;
    });
  },
  components: {
    'Modal': VueModal,
  },
  methods: {
    doShowMaskModal: function (doc) {
      console.log('>> ' + this.location);
      this.toShowInMaskModal = doc;
      this.showMaskModal = true;
    },
    doShowRiskModal: function (ix, head) {
      console.log('>> ' + this.location);
      this.showRiskModalIndex = ix;
      this.showRiskModalHead = head;
      this.showRiskModal = true;
    },
    doLocale: function (locn, redoHref) {
      let l = locn;
      switch (locn) {
        case 'ar':
          this.$i18n.locale = 'ar';
          break;
        case 'es':
          this.$i18n.locale = 'es';
          break;
        case 'th':
          this.$i18n.locale = 'th';
          break;
        case 'pt':
          this.$i18n.locale = 'pt';
          break;
        default:
          this.$i18n.locale = 'en';
          l = '';
      }
      this.location = this.$i18n.locale;
      if (redoHref) {
        window.location.href = `/#${l}`;
      }
    },
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
}

.table > :not(caption) > * > * {
  padding: 10px;
  border-bottom: 0;
  border-right: 1px solid white;
}

.table > :not(:first-child) {
  border: 1px solid white;
}

th {
  font-size: 20px;
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

table table {
  height: 72px;
}

.wrapper {
  position: relative;
}

.wrapper h1 {
  position: absolute;
}

#app[dir=ltr] .wrapper h1 {
  writing-mode: vertical-rl;
  text-orientation: mixed;
}

#app[dir=rtl] .wrapper h1 {
  writing-mode: vertical-rl;
  text-orientation: mixed;
}

#app[dir=ltr] .table-responsive-sm {
  padding-left: 30px;
}

#app[dir=rtl] .table-responsive-sm {
  padding-right: 54px;
}

@media screen and (max-width: 480px) {
  .table > :not(caption) > * > *, th {
    padding: 2px;
    font-size: 9px;
  }

  #app[dir=rtl] .wrapper h1, h1 {
    font-size: 14px;
  }
}

@media screen and (max-width: 600px) {
  #app[dir=rtl] .table-responsive-sm {
    padding-right: 33px;
  }
}

@media (min-width: 480px) and (max-width: 600px) {
  .table > :not(caption) > * > *, th {
    padding: 5px;
    font-size: 14px;
  }

  .wrapper h1 {
    font-size: 22px;
  }

  #app[dir=ltr] .table-responsive-sm {
    padding-left: 30px;
  }
}

@media (min-width: 480px) and (max-width: 520px) {
  #app[dir=ltr] .wrapper h1 {
    font-size: 19px;
  }
}

@media (min-width: 520px) and (max-width: 848px) {
  #app[dir=ltr] .wrapper h1 {
    font-size: 22px;
  }
}

@media (min-width: 480px) and (max-width: 800px) {
  #app[dir=rtl] .wrapper h1 {
    font-size: 23px;
  }
}

@media screen and (min-width: 600px) {
  #app[dir=ltr] .table-responsive-sm {
    padding-left: 40px;
  }
}

.modal-wrapper {
  display: flex;
  align-items: center;
}
.modal-wrapper .vm {
  top: auto;
}

</style>
