<template>
  <div class="jf-form">
    <the-tree
      v-for="item in orderProperty(schema)"
      :key="item.key + (typeof(uniqueKey) !== 'undefined' ? uniqueKey : '')"
      :uniqueKey = "uniqueKey"
      :objKey="[item.key]"
      :objVal="formValue[item.key]"
      v-bind="item.val"></the-tree>
    <!-- <div style="text-align: center; margin-top: 10px;">
      <Button @click="handleReset" type="warning">重置</Button>
      <Button @click="handleSubmit" type="primary">提交</Button>
    </div> -->
  </div>
</template>

<script>
import TheTree from './TheTree'
import Button from './button'
// utils
import { orderProperty, EventBus, cloneDeep } from '../utils'

// 表单验证这里参考iview的做法，通过mixin的方式将事件emitter注入到每个组件
// 通过事件的广播与派发，input组件blur之后向上dispatch事件，
// 然后在根组件统一负责表单的验证
export default {
  name: 'SchemaForm',
  components: {
    TheTree,
    Button
  },
  props: ['schema', 'value', 'uniqueKey'],
  created() {
    this.initFormData(this.value);
    EventBus.$on('on-form-item-add', field => {
      if(field)
      {
        if(field._props.uniqueKey === this.uniqueKey)
        {
          this.fields.push(field);
        }
      }
      return false;
    });
    EventBus.$on('on-form-item-remove', (field) => {
      if(field)
      {
        if(field._props.uniqueKey === this.uniqueKey)
        {
          this.fields.splice(this.fields.indexOf(field), 1)
        }
      }
      return false;
    });
    EventBus.$on('on-set-form-data', payload => {
      if(payload.uniqueKey === this.uniqueKey)
      {
        this.setFormData(payload);
      }
    });
  },
  methods: {
    initFormData(value) {
        this.orderProperty(this.schema).map(item=> {
          Object.keys(item.val.properties).map(key => {
            if (item.val.properties[key]['type'] === 'TheObject' && item.val.properties[key].hasOwnProperty('columns')
            && Object.keys(item.val.properties[key]['columns']).length > 0) {
              Object.keys(item.val.properties[key]['columns']).map(col => {
                if (value[item.key] && value[item.key][key]
                && !value[item.key][key].hasOwnProperty(col)) {
                  // console.log('printing schema for col',JSON.stringify(item.val.properties[key]['addDefault'][col]));
                  // console.log('check if prop ('+ col +') is present', JSON.stringify(value[item.key][key]))

                  //alternative 1, set Value based on column type
                  // value[item.key][key][col] = item.val.properties[key]['columns'][col]['type']==='TheSelect'? null : '';

                  //alternative 2, set Value from addDefault
                  value[item.key][key][col] = item.val.properties[key]['addDefault'][col];
                }
              })
            }
          })
        })
        this.formValue = JSON.parse(JSON.stringify(value));
    },
    cloneDeep(source) {
      return cloneDeep(source);
    },
    setFormData(payload) {
        const { key, value } = payload;
        key.reduce((pre, cur, curIndex, arr) => {
            // 如果是最后一项，就是我们要改变的字段
            if(curIndex === arr.length - 1) {
                // Vue 不能检测直接用索引设置数组某一项的值
                if(typeof(cur) === 'number') {
                    return pre.splice(cur, 1, value);
                } else {
                    return pre[cur] = value;
                }
            }
            return pre[cur] = pre[cur] || {}
        }, this.formValue);
    },
    validate(cb) {
      let err = false;
      let len = this.fields.length;
      this.fields.forEach((field, index) => {
        field.validate().then(res => {
          const { title, status } = res;
          if(!status) {
            err = true;
          }
          if((index + 1) === len) {
            cb(err, this.formValue);
          }
        }).catch(err => {
          console.log(err);
        })
      })
    },
    // 三种思路，第一种方式在每个组件保存初始值，reset的时候循环调用每一个reset事件
    // 第二种思路，根组件保存一个深拷贝的初始值，然后统一reset
    // 第二种思路只能恢复初始值，没法处理组件内的状态，比如校验信息
    // 第三种思路结合两种思路，在根组件reset数据，在每个组件内部处理自己的状态
    resetFields() {
      this.initFormData(this.value);
      this.fields.forEach(field => {
        field.resetField && field.resetField();
      });
    },
     // 根据propertyOrder 从小到大排序
    orderProperty(obj) {
      return orderProperty(obj);
    },
    refreshFormData(){
      this.initFormData(this.value);
    },
    getFormValue(){
      return this.formValue;
    }
  },
  watch :{
    'schema' : function(){
      this.initFormData(this.formValue);
    },
  },
  data () {
    return {
      "initFormValue": {},
      "formValue": {},
      "fields": []
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="postcss">
  @import '../../node_modules/bootstrap/dist/css/bootstrap.css';
  @import '../../node_modules/bootstrap-vue/dist/bootstrap-vue.css';
  @import '../../node_modules/@riophae/vue-treeselect/dist/vue-treeselect.css';
  @import '../../node_modules/simplemde/dist/simplemde.min.css';
  @import "../styles/base.css";
  @import "../styles/style.css";
</style>
