<template>
  <div v-if="typeof(controlOptions) !== 'undefined' && controlOptions.readOnly">
    <b-form-group :label="!noLabel ? title : ''">
      <p class="form-value">{{ this.getLabel }}</p>
    </b-form-group>
  </div>
  <div v-else>
    <b-form-group>
      <template slot="label">
        <p class="mb-0">{{ !this.noLabel ? this.title : '' }}</p>
        <small>{{this.noDescription ? '' : this.controlOptions.description}}</small>
      </template>
      <toggle-button :value="false" v-model="msg" v-if="options.length === 0" :labels="toggleSwitch.labels" :width="toggleSwitch.width" @change="handleChange"/>
      <b-form-radio-group v-if="options.length > 0" v-model="msg" :checked="msg" @change="handleChange">
        <b-form-radio :value="item.value" v-for="item in options" :key="item.value">{{item.label}}</b-form-radio>
      </b-form-radio-group>
      <small class="error text-danger" v-if="showValidate">{{validateInfo}}</small>
    </b-form-group>
  </div>
</template>

<script>

// utils
import { EventBus } from '../utils'

//mixin
import Base from '../mixins/base';
import Validate from '../mixins/validate';
import { ToggleButton } from 'vue-js-toggle-button'


export default {
  name: 'TheRadio',
  mixins: [ Validate, Base ],
  props: ['options', 'title', 'objKey', 'objVal', 'noLabel', 'rules', 'validateObj', 'keyArr', 'parentName', 'callBackEvent', 'controlOptions', 'uniqueKey', 'toggleSwitch'],
  methods: {
    handleChange() {
      this.asyncValidate();
    }
  },
  components: {
    ToggleButton
  },
  data () {
    return {
      keyName: this.objKey,
      validateState: '',
      validateMessage: ''
    }
  },
  computed : {
    noDescription(){
      return !(typeof this.controlOptions !== 'undefined' && this.controlOptions.hasOwnProperty('description'))
    },
    getLabel() {
      return (this.msg) ? this.msg : '';
    }
  },
  created(){
    if(typeof(this.callBackEvent) === 'function')
    {
      this.callBackEvent(this.objKey, this.objVal, true, this.uniqueKey);
    }
  },
  watch : {
    "objVal" : function(params) {
        if(typeof(this.callBackEvent) === 'function')
        {
          this.callBackEvent(this.objKey, this.objVal,true, this.uniqueKey);
        }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style lang="postcss">
  @import "../styles/radio.css";
</style>
