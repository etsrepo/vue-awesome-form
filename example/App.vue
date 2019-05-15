<template>
  <div id="app">
    <img src="../src/assets/logo.png">
    <div class="container pt-5 pb-5">
      <schema-form
        ref="schemaForm"
        :schema="getSchema"
        :value="getValue"
        :key = "F1"
        :uniqueKey = "1"
      ></schema-form>
      <div class="text-center mt-4">
        <button @click="handleReset" type="button" class="btn btn-default">
          <span>Reset</span>
        </button>
        <button @click="handleSubmit" type="button" class="btn btn-primary">
          <span>Submit</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
// import SchemaForm from '../dist/vue-json-form.js'
// import SchemaForm from '../src/main.js'


export default {
  name: 'App',
  methods: {
    handleSubmit() {
      this.$refs.schemaForm.validate((err, values) => {
        if(err) {
          console.log('验证失败 :: Not Valid');
        } else {
          let misMatchedKeys = Object.keys(values.trans.attributes).map(valueCol => {
              if(!this.addDefault.hasOwnProperty(valueCol) && valueCol !== 'enabled' && valueCol !== 'transformationName') {
                delete values.trans.attributes[valueCol];
              }
          })
          // console.log('mismatch', JSON.stringify(misMatchedKeys))
          console.log('验证成功 :: Valid', JSON.stringify(values));
        }
      })
    },
    onTransformChange(objKey, objVal, isFirstTime){
        const property = 'transformationName';
        // console.log('before schema change::', JSON.stringify(this.formData.value))
        if(typeof objVal !== 'undefined' && objVal !== '' && objVal) {
            this.addDefault = {};
            this.attributesSchema = this.allAttributesSchema[objVal];
            this.addDefault = this.defaultAttributeSchema[objVal];

            console.log('transformation change', JSON.stringify(this.$refs.schemaForm.getFormValue()))
            let formValue = this.$refs.schemaForm.getFormValue();
            this.localData.attributes = formValue.trans.attributes;
            Object.keys(this.localData.attributes).map(valueCol => {
                if(this.addDefault.hasOwnProperty(valueCol) && valueCol !== 'enabled'
                && valueCol !== 'transformationName') {
                    this.localData.attributes[valueCol] = '';
                }
            })
            // this.$refs.schemaForm.initFormData(this.$refs.schemaForm.values)
            // this.addDefault.enabled  = false;
            // console.log('GET_DATA',JSON.stringify(this.formData.value));
            // console.log('GET_SCHEMA',JSON.stringify(this.formData.schema));
        } else {
            this.attributesSchema = {};
        }
    },
    handleReset() {
      this.$refs.schemaForm.resetFields();
    },
    interestsCallBack(objKey, objVal) {
      // console.log(JSON.stringify(objKey), ' inside interestsCallBack::', JSON.stringify(objVal));
    },
    numberChanged(objKey, objVal){
      console.log(JSON.stringify(objKey), 'Text Box callback::', JSON.stringify(objVal));
    },
    genderCallback(objKey, objVal){
      // console.log('objVal inside genderCallback::', objVal);
      // let val = objKey.filter(k => k !== 'gender')
      //         .reduce((obj, k) => obj[k], this.formData.value.register.sample.pets);
      if(objVal){
        // this.formData.schema.register.properties.pets.columns['name'] = {
        //   "type": "TheInput",
        //   "title": "Name",
        //   // "callBackEvent": this.focusOut,
        //   "rules": {
        //     "required": true,
        //     "message": "Name is required"
        //   },
        //   "layoutClass":"col-lg-12"
        // }
        // this.formData.schema.register.properties.pets.columns = Object.assign({}, this.formData.schema.register.properties.pets.columns);
      }else{
        // delete this.formData.schema.register.properties.pets.columns.name;
        // this.formData.schema.register.properties.pets.columns = Object.assign({}, this.formData.schema.register.properties.pets.columns);
      }
    }
  },
  data () {
    return {
      "localData": {
          attributes: {
            "transformationName": null,
            "enabled": ''
          }
      },
      "attributesSchema": {},
      "addDefault": {},
      "allAttributesSchema": {"Delete":
      {"HTMLSelector":
        { "type":"TheInput",
          "readOnly": true,
          "title":"HTML Selector",
          "layoutClass":"col-md-6",
          "rules":
            { 
            "required":false,
            "message":"This field is required"
            }
          }
      },
      "Upsert":{"HTMLSelector":{"type":"TheInput","title":"HTML Selector","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"AttributeList":{"type":"TheInput","title":"Attribute List","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}}},"StringReplace":{"SearchString":{"type":"TheInput","title":"SearchString","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"ReplaceString":{"type":"TheInput","title":"ReplaceString","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}}},"ExcludeStaticAssetHostNames":{},"ProgressiveRenderExisting":{"HTMLSelector":{"type":"TheInput","title":"HTML Selector","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"DestinationURL":{"type":"TheInput","title":"Destination URL","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"NVSwapIdPrefix":{"type":"TheInput","title":"NV Swap Id Prefix","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}}},"RelocateTag":{"HTMLSelector":{"type":"TheInput","title":"HTML Selector","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"ParentTag":{"type":"TheInput","title":"Parent Tag","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"NthChild":{"type":"TheSelect","title":"Nth Child","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"},"options":[{"id":"first","label":"first"},{"id":"last","label":"last"}]}},"DeleteDuplicateTagsWithSameInnerHTML":{"HTMLSelector":{"type":"TheInput","title":"HTML Selector","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"ValidTagPosition":{"type":"TheInput","title":"Valid Tag Position","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"DeleteLastOccurance":{"type":"TheSelect","title":"Delete Last Occurance","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"},"options":[{"id":"false","label":"false"},{"id":"true","label":"true"}]}},"ExcludeRequestParameters":{},"CopyAttributeValueSameTagIfExist":{"HTMLSelector":{"type":"TheInput","title":"HTML Selector","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"SourceAttribute":{"type":"TheInput","title":"Source Attribute","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"TargetAttribute":{"type":"TheInput","title":"Target Attribute","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}}},"RemoveSpecialChars":{},"DeleteTagContainsSameAttributeWithValue":{"HTMLSelector":{"type":"TheInput","title":"HTML Selector","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"AttributeName":{"type":"TheInput","title":"Attribute Name","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}}},"ReplaceAdditionalBaseURL":{},"AddHtmlizedComment":{},"MoveInsert":{"HTMLSelector":{"type":"TheInput","title":"HTML Selector","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"ContentTemplatePath":{"type":"TheInput","title":"Content Template Path","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"DestinationPath":{"type":"TheInput","title":"Destination Path","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}}},"Move":{"HTMLSelector":{"type":"TheInput","title":"HTML Selector","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"DestinationPath":{"type":"TheInput","title":"Destination Path","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}}},"ProgressiveRender":{"HTMLSelector":{"type":"TheInput","title":"HTML Selector","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"DestinationURL":{"type":"TheInput","title":"Destination URL","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"NVSwapIdPrefix":{"type":"TheInput","title":"NV Swap Id Prefix","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"DestinationPath":{"type":"TheInput","title":"Destination Path","layoutClass":"col-md-6","rules":{"required":false,"message":"This field is required"}}},"AddDirectoryPath":{},"ParseCss":{},"SwitchParentChildTag":{"HTMLSelector":{"type":"TheInput","title":"HTML Selector","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"ParentTag":{"type":"TheInput","title":"Parent Tag","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"NthTag":{"type":"TheInput","title":"Nth Tag","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}}},"ReplaceMultiLineWithSingleLine":{"HTMLSelector":{"type":"TheInput","title":"HTML Selector","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}}},"DeleteInsert":{"HTMLSelector":{"type":"TheInput","title":"HTML Selector","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"ContentTemplatePath":{"type":"TheInput","title":"Content Template Path","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}}},"CopyAttributeValueSameTag":{"HTMLSelector":{"type":"TheInput","title":"HTML Selector","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"SourceAttribute":{"type":"TheInput","title":"Source Attribute","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}},"TargetAttribute":{"type":"TheInput","title":"Target Attribute","layoutClass":"col-md-6","rules":{"required":true,"message":"This field is required"}}},"ExcludeDomainURL":{}},
      "defaultAttributeSchema": {"Delete":{"HTMLSelector":""},"Upsert":{"HTMLSelector":"","AttributeList":""},"StringReplace":{"SearchString":"","ReplaceString":"","transformationName":null,"enabled":false},"ExcludeStaticAssetHostNames":{},"ProgressiveRenderExisting":{"HTMLSelector":"","DestinationURL":"","NVSwapIdPrefix":""},"RelocateTag":{"HTMLSelector":"","ParentTag":"","NthChild":null},"DeleteDuplicateTagsWithSameInnerHTML":{"HTMLSelector":"","ValidTagPosition":"","DeleteLastOccurance":null},"ExcludeRequestParameters":{},"CopyAttributeValueSameTagIfExist":{"HTMLSelector":"","SourceAttribute":"","TargetAttribute":""},"RemoveSpecialChars":{},"DeleteTagContainsSameAttributeWithValue":{"HTMLSelector":"","AttributeName":""},"ReplaceAdditionalBaseURL":{},"AddHtmlizedComment":{},"MoveInsert":{"HTMLSelector":"","ContentTemplatePath":"","DestinationPath":""},"Move":{"HTMLSelector":"","DestinationPath":""},"ProgressiveRender":{"HTMLSelector":"","DestinationURL":"","NVSwapIdPrefix":"","DestinationPath":""},"AddDirectoryPath":{},"ParseCss":{},"SwitchParentChildTag":{"HTMLSelector":"","ParentTag":"","NthTag":""},"ReplaceMultiLineWithSingleLine":{"HTMLSelector":""},"DeleteInsert":{"HTMLSelector":"","ContentTemplatePath":""},"CopyAttributeValueSameTag":{"HTMLSelector":"","SourceAttribute":"","TargetAttribute":""},"ExcludeDomainURL":{}},
      "F1": "",
      "formData": {
        "value": {

            "register": {
              "pets": [
                    {
                    "type": null,
                    "name": "",
                    "gender": false,
                    "numbers": 1000,
                    // "interests": [{
                    //   "value": 200,
                    //   "color": "#B5F8FE"
                    // }, {
                    //   "value": 201,
                    //   "color": "#B55F22"
                    // }],
                    // "interests": {
                    //   "interest": [1], "interestColor": ["#B5F8FE"]
                    // },
                    // "interests": [200, 201],
                    "interests": ["200_#B5F8FE", "201_#B55F22"],
                    // "interests": [],
                    "roles" : ["Hello", "There"],
                    "description" : "",
                    "proxy" : {
                      "name" : "",
                      "products" : ""
                    },
                    "attributes": []
                  },
                  // {
                  //   "type": "fish",
                  //   "name": "",
                  //   "gender": true,
                  //   "numbers": "",
                  //   "interests": ["200_#B5F8FE", "201_#B55F22"],
                  //   // "interests": [],
                  //   "roles" : ["HI", "TWO"],
                  //   "description" : "",
                  //   "proxy" : {
                  //     "name" : "",
                  //     "products" : ""
                  //   },
                  //   attributes: []
                  // },{
                  //   "type": "dog",
                  //   "name": "",
                  //   "gender": false,
                  //   "numbers": "",
                  //   "interests": ["200_#B5F8FE", "201_#B55F22"],
                  //   // "interests": [],
                  //   "roles" : ["HI", "TWO"],
                  //   "description" : "",
                  //   "proxy" : {
                  //     "name" : "",
                  //     "products" : ""
                  //   },
                  //   attributes: []
                  // }
                  ],
                "sample" : {
                  name: "",
                  // "testDefault": "",

                }
            },
        }
      },
    }
  },
  computed : {
    getValue() {
      return {
        trans: this.localData
      }
     
    },
    readOnly(){
       return {disabled: true}
     },
    getSchema(){
      let schemaToReturn = {
        "trans": {
            "type": "TheTree",
            "title": "",
            "properties": {
              "attributes": {
                "type": "TheObject",
                "title": "",
                "columns": {
                  "transformationName": {
                    "type": "TheSelect",
                    "title": "Transformation Name",
                    "callBackEvent" : this.onTransformChange,
                    "options": [
                      {
                        "id": "Delete",
                        "label": "Delete"
                      },
                      {
                        "id": "Upsert",
                        "label": "Upsert"
                      },
                      {
                        "id": "StringReplace",
                        "label": "StringReplace"
                      },
                      {
                        "id": "ExcludeStaticAssetHostNames",
                        "label": "ExcludeStaticAssetHostNames"
                      },
                      {
                        "id": "ProgressiveRenderExisting",
                        "label": "ProgressiveRenderExisting"
                      },
                      {
                        "id": "RelocateTag",
                        "label": "RelocateTag"
                      },
                      {
                        "id": "DeleteDuplicateTagsWithSameInnerHTML",
                        "label": "DeleteDuplicateTagsWithSameInnerHTML"
                      },
                      {
                        "id": "ExcludeRequestParameters",
                        "label": "ExcludeRequestParameters"
                      },
                      {
                        "id": "CopyAttributeValueSameTagIfExist",
                        "label": "CopyAttributeValueSameTagIfExist"
                      },
                      {
                        "id": "RemoveSpecialChars",
                        "label": "RemoveSpecialChars"
                      },
                      {
                        "id": "DeleteTagContainsSameAttributeWithValue",
                        "label": "DeleteTagContainsSameAttributeWithValue"
                      },
                      {
                        "id": "ReplaceAdditionalBaseURL",
                        "label": "ReplaceAdditionalBaseURL"
                      },
                      {
                        "id": "AddHtmlizedComment",
                        "label": "AddHtmlizedComment"
                      },
                      {
                        "id": "MoveInsert",
                        "label": "MoveInsert"
                      },
                      {
                        "id": "Move",
                        "label": "Move"
                      },
                      {
                        "id": "ProgressiveRender",
                        "label": "ProgressiveRender"
                      },
                      {
                        "id": "AddDirectoryPath",
                        "label": "AddDirectoryPath"
                      },
                      {
                        "id": "ParseCss",
                        "label": "ParseCss"
                      },
                      {
                        "id": "SwitchParentChildTag",
                        "label": "SwitchParentChildTag"
                      },
                      {
                        "id": "ReplaceMultiLineWithSingleLine",
                        "label": "ReplaceMultiLineWithSingleLine"
                      },
                      {
                        "id": "DeleteInsert",
                        "label": "DeleteInsert"
                      },
                      {
                        "id": "CopyAttributeValueSameTag",
                        "label": "CopyAttributeValueSameTag"
                      },
                      {
                        "id": "ExcludeDomainURL",
                        "label": "ExcludeDomainURL"
                      }
                    ],
                    "rules": {
                      "required": true,
                      "message": "This field is required"
                    }
                  },
                  "enabled": {
                    "type": "TheCheckbox",
                    "title": "Enabled",
                    "options": [
                      {
                        "label": "",
                        "value": true
                      }
                    ],
                    "rules": {
                      "required": true,
                      "message": "This field is required"
                    }
                  },
                  ...this.attributesSchema
                },
                "addDefault": {
                  ...this.addDefault
                }
              }
            },
            "controlOptions": {
              "readOnly": false
            }
          }
      }
      return schemaToReturn;
          // "register": {
          //       "type": "TheTree",
          //       "title": "Register",
          //       "properties": {
          //           "pets": {
          //                 "type": "TheTable",
          //                 "title": "",
          //                 "columns": {
          //                   numbers: {
          //                     type : "TheInput",
          //                     title : "Random Numbers",
          //                     // theFormat : "number",
          //                     callBackEvent: this.numberChanged,
          //                     rules : {
          //                       required : true,
          //                       message : "This field is required"
          //                     },
          //                     layoutClass : "col-lg-12"
          //                   },
          //                   password: {
          //                     type : "TheInput",
          //                     title : "Password",
          //                     theFormat : "password",
          //                     rules : {
          //                       required : false,
          //                       message : "This field is required"
          //                     },
          //                     layoutClass : "col-lg-12"
          //                   },
          //                   "type": {
          //                     "type": "TheSelect",
          //                     "title": "Type",
          //                     // "propertyOrder": 1,
          //                     // "controlOptions" : {

          //                     // },
          //                     "options": [
          //                       {
          //                         "id": "cat",
          //                         "label": "Cat",
          //                       },
          //                       {
          //                         "id": "dog",
          //                         "label": "Dog",
          //                         "class" : "dog-icon"
          //                       },
          //                       {
          //                         "id": "fish",
          //                         "label": "Fish",
          //                         "class" : "fish-icon"
          //                       },
          //                     ],
          //                     "rules": {
          //                       // "type": "number",
          //                       "required": true,
          //                       "message": "Select valid value"
          //                     },

          //                   },
          //                   "gender": {
          //                     "type": "TheRadio",
          //                     "title": "Gender",
          //                     // "propertyOrder": 3,
          //                     "toggleSwitch": {
          //                       "width": 65,
          //                       "labels": {
          //                         "checked": "Male",
          //                         "unchecked": "Female"
          //                       }
          //                     },
          //                     "options": [],
          //                     // "options": [
          //                     //   {
          //                     //     "value": true,
          //                     //     "label": "Male"
          //                     //   },
          //                     //   {
          //                     //     "value": false,
          //                     //     "label": "Female"
          //                     //   }
          //                     // ],
          //                     "callBackEvent" : this.genderCallback,
          //                     "rules": {
          //                       "type": "boolean",
          //                       "required": true,
          //                       "message": "The gender cannot be empty"
          //                     }
          //                   },
          //                   // "newname": {
          //                   //   "type": "TheInput",
          //                   //   "title": "Name",
          //                   //   // "callBackEvent": this.focusOut,
          //                   //   "rules": {
          //                   //     "type": "string",
          //                   //     "required": true,
          //                   //     "message": "Name is required"
          //                   //   },
          //                   //   "layoutClass":"col-lg-12"
          //                   // },
          //                   // "interests": {
          //                   //   "type": "TheObject",
          //                   //   "title": "",
          //                   //   "propertyOrder": 2,
          //                   //   "columns": {
          //                   //     "interest": {
          //                   //       "type": "TheCheckbox",
          //                   //       "title": "Interests",
          //                   //       "propertyOrder": 1,
          //                   //       "options": [
          //                   //         {
          //                   //           "value": 1,
          //                   //           "label": "Badminton",
          //                   //         },
          //                   //         {
          //                   //           "value": 2,
          //                   //           "label": "Hockey",
          //                   //         },
          //                   //         {
          //                   //           "value": 3,
          //                   //           "label": "Tennis",
          //                   //         }
          //                   //       ],
          //                   //       "callBackEvent" : this.interestsTextCallBack,
          //                   //       "rules": {
          //                   //         "type": "array",
          //                   //         "required": true,
          //                   //         "message": "The interests cannot be empty"
          //                   //       }
          //                   //     },
          //                   //     "interestColor": {
          //                   //       "type": "TheColorSwatch",
          //                   //       "title": "Interest Colors",
          //                   //       "propertyOrder": 2,
          //                   //       "propertyOrder": 2,
          //                   //       "options": [
          //                   //         {
          //                   //           "value": "#EDEDED",
          //                   //           "label": "#EDEDED"
          //                   //         },
          //                   //         {
          //                   //           "value": "#EDEDED",
          //                   //           "label": "#EDEDED"
          //                   //         },
          //                   //         {
          //                   //           "value": "#EDEDED",
          //                   //           "label": "#EDEDED"
          //                   //         }
          //                   //       ],
          //                   //       "callBackEvent" : this.interestsColorsCallBack,
          //                   //     },
          //                   //     "rules": {
          //                   //       "type": "array",
          //                   //       "required": true,
          //                   //       "message": "The interests cannot be empty"
          //                   //     }
          //                   //   }
          //                   // },
          //                   "interests": {
          //                     "type": "TheCheckbox",
          //                     "title": "Interests",
          //                     // "propertyOrder": 2,
          //                     // "optionsColor": false,
          //                     // "options": [
          //                     //   {
          //                     //     "value": 200,
          //                     //     "label": "200"
          //                     //   },
          //                     //   {
          //                     //     "value": 201,
          //                     //     "label": "202"
          //                     //   },
          //                     //   {
          //                     //     "value": 300,
          //                     //     "label": "300"
          //                     //   }
          //                     // ],
          //                     "options": [
          //                       {
          //                         "value": 200,
          //                         "label": "200",
          //                         "color": "#EDEDED"
          //                       },
          //                       {
          //                         "value": 201,
          //                         "label": "201",
          //                         "color": "#EDEDED"
          //                       },
          //                       {
          //                         "value": 300,
          //                         "label": "300",
          //                         "color": "#EDEDED"
          //                       }
          //                     ],
          //                     "callBackEvent" : this.interestsCallBack,
          //                     "rules": {
          //                       "type": "array",
          //                       "required": true,
          //                       "message": "The interests cannot be empty"
          //                     }
          //                   },
          //                   "roles" : {
          //                       "type": "TheAddInput",
          //                       "title": "Roles",
          //                       "rules": {
          //                           "required": true,
          //                           "message": "This field is required"
          //                       },
          //                       "layoutClass":"col-md-4",
          //                       "controlOptions" : {
          //                         "isTags" : true,
          //                       },
          //                   },
          //                   // "description": {
          //                   //     "type": "TheTextArea",
          //                   //     "title": "Description",
          //                   //     "rules": [{
          //                   //         "required": true,
          //                   //         "message": "This field is required"
          //                   //     }],
          //                   //     "layoutClass" : "col-lg-12",
          //                   //     "controlOptions" : {
          //                   //       "isMD" : true,
          //                   //     },
          //                   // },
          //                   // "proxy" : {
          //                   //   "type": "TheObject",
          //                   //   "title": "Proxy",
          //                   //   "columns": {
          //                   //     "name" : {
          //                   //       "type": "TheInput",
          //                   //       "title": "Name",
          //                   //       "rules": {},
          //                   //       "layoutClass" : "col-lg-12",
          //                   //     },
          //                   //     "products" : {
          //                   //       "type": "TheInput",
          //                   //       "title": "Products",
          //                   //       "rules": {},
          //                   //       "layoutClass" : "col-lg-12",
          //                   //     },
          //                   //   },
          //                   //   "controlOptions" : {
          //                   //     "disabled" : true,
          //                   //   },
          //                   // }
          //                   attributes: {
          //                     type : "TheTable",
          //                     title : "Group Attributes",
          //                     columns : {
          //                         labelName : {
          //                             type : "TheInput",
          //                             title : "Label Name",
          //                             rules : {
          //                                 required : true,
          //                                 message : "This field is required"
          //                             },
          //                             layoutClass : "col-lg-12"
          //                         },
          //                         labelValue : {
          //                             type : "TheInput",
          //                             title : "Label Value",
          //                             rules : {
          //                                 required : true,
          //                                 message : "This field is required"
          //                             },
          //                             layoutClass : "col-lg-12"
          //                         },
          //                         textBox: {
          //                             type: "TheRadio",
          //                             title : "Text Box",
          //                             toggleSwitch: {
          //                                 width: 65,
          //                                 labels: {
          //                                     checked: "Active",
          //                                     unchecked: "Inactive"
          //                                 }
          //                             },
          //                             options: [],
          //                             rules : {
          //                                 type : 'boolean',
          //                                 required : true,
          //                                 message : "This field is required"
          //                             },
          //                             layoutClass : "col-lg-12"
          //                         },
          //                         radioButton: {
          //                             type: "TheRadio",
          //                             title : "Radio Button",
          //                             toggleSwitch: {
          //                                 width: 65,
          //                                 labels: {
          //                                     checked: "Active",
          //                                     unchecked: "Inactive"
          //                                 }
          //                             },
          //                             options: [],
          //                             rules : {
          //                                 type : 'boolean',
          //                                 required : true,
          //                                 message : "This field is required"
          //                             },
          //                             layoutClass : "col-lg-12"
          //                         },
          //                         dropDown: {
          //                             type: "TheRadio",
          //                             title : "Text Box",
          //                             toggleSwitch: {
          //                                 width: 65,
          //                                 labels: {
          //                                     checked: "Active",
          //                                     unchecked: "Inactive"
          //                                 }
          //                             },
          //                             options: [],
          //                             rules : {
          //                                 type : 'boolean',
          //                                 required : true,
          //                                 message : "This field is required"
          //                             },
          //                             layoutClass : "col-lg-12"
          //                         },
          //                         required: {
          //                             type: "TheRadio",
          //                             title : "Text Box",
          //                             toggleSwitch: {
          //                                 width: 65,
          //                                 labels: {
          //                                     checked: "True",
          //                                     unchecked: "False"
          //                                 }
          //                             },
          //                             options: [],
          //                             rules : {
          //                                 type : 'boolean',
          //                                 required : true,
          //                                 message : "This field is required"
          //                             },
          //                             layoutClass : "col-lg-12"
          //                         }
          //                     },
          //                     layoutClass : "col-lg-12",
          //                     controlOptions : {
          //                         isTab : true,
          //                         tabTitle : "Group Attribute"
          //                     },
          //                     addDefault: {
          //                       "labelName": "",
          //                       "labelValue": "",
          //                       "textBox": true,
          //                       "radioButton": false,
          //                       "dropDown": false,
          //                       "required": true
          //                     },
          //                     rules: {
          //                       required: true,
          //                       message: 'Please add Group attr'
          //                     }
          //                   }
          //                 },
          //                 addDefault: {
          //                   type: null,
          //                   name: '',
          //                   gender: true,
          //                   numbers: '',
          //                   attributes: []
          //                 },
          //                 "addText": "+ Add Pet",
          //                 "readOnlyText": "Pets",
          //                 "rules": {
          //                   "type": "array",
          //                   "required": true,
          //                   "message": "The pets cannot be empty"
          //                 },
          //                 "controlOptions" : {
          //                   "isTab" : true,
          //                   "sortable" : true,
          //                   "tabTitle" : "Pet",
          //                   "displayIndex": 2
          //                 },
          //                 "layoutClass" : "col-lg-12",
          //               },
          //           "sample" : {
          //             "type" : "TheObject",
          //             "title" : "",
          //             "columns" : {
          //               "name": {
          //                 "type": "TheInput",
          //                 "title": "Name",
          //                 "rules": {
          //                   "required": true,
          //                   "message": "this is mandatory"
          //                 }
          //               },
          //               "testDefault": {
          //                 "type": "TheInput",
          //                 "title": "Hey There",
          //                 "rules": {
          //                   "required": true,
          //                   "message": "this is mandatory"
          //                 }
          //               },


          //             },
          //             "layoutClass" : "col-lg-12",
          //             "addDefault" : {
          //               "testDefault": ""
          //             }
          //           }
          //       },
          //       "controlOptions" : {
          //         "readOnly" : true
          //       },
          //   }
    }
  }
}
</script>