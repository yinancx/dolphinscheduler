/*
 * Licensed to the Apache Software Foundation (ASF) under one or more
 * contributor license agreements.  See the NOTICE file distributed with
 * this work for additional information regarding copyright ownership.
 * The ASF licenses this file to You under the Apache License, Version 2.0
 * (the "License"); you may not use this file except in compliance with
 * the License.  You may obtain a copy of the License at
 *
 *    http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
<template>
  <div class="spark-model">
    <m-list-box>
      <div slot="text">{{$t('Program Type')}}</div>
      <div slot="content">
        <el-select
                style="width: 130px;"
                size="small"
                v-model="programType"
                :disabled="isDetails">
          <el-option
                  v-for="city in programTypeList"
                  :key="city.code"
                  :value="city.code"
                  :label="city.code">
          </el-option>
        </el-select>
      </div>
    </m-list-box>
    <m-list-box>
      <div slot="text">{{$t('Spark Version')}}</div>
      <div slot="content">
        <el-select
                style="width: 130px;"
                size="small"
                v-model="sparkVersion"
                :disabled="isDetails">
          <el-option
                  v-for="city in sparkVersionList"
                  :key="city.code"
                  :value="city.code"
                  :label="city.code">
          </el-option>
        </el-select>
      </div>
    </m-list-box>
    <m-list-box v-if="programType !== 'PYTHON'">
      <div slot="text">{{$t('Main Class')}}</div>
      <div slot="content">
        <el-input
            :disabled="isDetails"
            type="input"
            size="small"
            v-model="mainClass"
            :placeholder="$t('Please enter main class')">
        </el-input>
      </div>
    </m-list-box>
    <m-list-box>
      <div slot="text">{{$t('Main Jar Package')}}</div>
      <div slot="content">
        <treeselect v-model="mainJar" maxHeight="200" :options="mainJarLists" :disable-branch-nodes="true" :normalizer="normalizer" :disabled="isDetails" :placeholder="$t('Please enter main jar package')">
          <div slot="value-label" slot-scope="{ node }">{{ node.raw.fullName }}</div>
        </treeselect>
      </div>
    </m-list-box>
    <m-list-box>
      <div slot="text">{{$t('Deploy Mode')}}</div>
      <div slot="content">
        <el-radio-group v-model="deployMode" size="small">
          <el-radio :label="'cluster'" :disabled="isDetails"></el-radio>
          <el-radio :label="'client'" :disabled="isDetails"></el-radio>
          <el-radio :label="'local'" :disabled="isDetails"></el-radio>
        </el-radio-group>
      </div>
    </m-list-box>
    <m-list-box>
      <div slot="text">{{$t('App Name')}}</div>
      <div slot="content">
        <el-input
          :disabled="isDetails"
          type="input"
          size="small"
          v-model="appName"
          :placeholder="$t('Please enter app name(optional)')">
        </el-input>
      </div>
    </m-list-box>
    <m-list-4-box>
      <div slot="text">{{$t('Driver Cores')}}</div>
      <div slot="content">
        <el-input
          :disabled="isDetails"
          type="input"
          size="small"
          v-model="driverCores"
          :placeholder="$t('Please enter Driver cores')">
        </el-input>
      </div>
      <div slot="text-2">{{$t('Driver Memory')}}</div>
      <div slot="content-2">
        <el-input
          :disabled="isDetails"
          type="input"
          size="small"
          v-model="driverMemory"
          :placeholder="$t('Please enter Driver memory')">
        </el-input>
      </div>
    </m-list-4-box>
    <m-list-4-box>
      <div slot="text">{{$t('Executor Number')}}</div>
      <div slot="content">
        <el-input
          :disabled="isDetails"
          type="input"
          size="small"
          v-model="numExecutors"
          :placeholder="$t('Please enter Executor number')">
        </el-input>
      </div>
      <div slot="text-2">{{$t('Executor Memory')}}</div>
      <div slot="content-2">
        <el-input
          :disabled="isDetails"
          type="input"
          size="small"
          v-model="executorMemory"
          :placeholder="$t('Please enter Executor memory')">
        </el-input>
      </div>
    </m-list-4-box>
    <m-list-4-box>
      <div slot="text">{{$t('Executor Cores')}}</div>
      <div slot="content">
        <el-input
          :disabled="isDetails"
          type="input"
          size="small"
          v-model="executorCores"
          :placeholder="$t('Please enter Executor cores')">
        </el-input>
      </div>
    </m-list-4-box>
    <m-list-box>
      <div slot="text">{{$t('Main Arguments')}}</div>
      <div slot="content">
        <el-input
            :autosize="{minRows:2}"
            :disabled="isDetails"
            type="textarea"
            size="small"
            v-model="mainArgs"
            :placeholder="$t('Please enter main arguments')">
        </el-input>
      </div>
    </m-list-box>
    <m-list-box>
      <div slot="text">{{$t('Option Parameters')}}</div>
      <div slot="content">
        <el-input
            :disabled="isDetails"
            :autosize="{minRows:2}"
            type="textarea"
            size="small"
            v-model="others"
            :placeholder="$t('Please enter option parameters')">
        </el-input>
      </div>
    </m-list-box>
    <m-list-box>
      <div slot="text">{{$t('Resources')}}</div>
      <div slot="content">
        <treeselect v-model="resourceList" :multiple="true" maxHeight="200" :options="mainJarList" :normalizer="normalizer" :value-consists-of="valueConsistsOf" :disabled="isDetails" :placeholder="$t('Please select resources')">
          <div slot="value-label" slot-scope="{ node }">{{ node.raw.fullName }}<span  class="copy-path" @mousedown="_copyPath($event, node)" >&nbsp; <em class="el-icon-copy-document" data-container="body"  data-toggle="tooltip" :title="$t('Copy path')" ></em> &nbsp;  </span></div>
        </treeselect>
      </div>
    </m-list-box>
    <m-list-box>
      <div slot="text">{{$t('Custom Parameters')}}</div>
      <div slot="content">
        <m-local-params
                ref="refLocalParams"
                @on-local-params="_onLocalParams"
                :udp-list="localParams"
                :hide="false">
        </m-local-params>
      </div>
    </m-list-box>
  </div>
</template>
<script>
  import _ from 'lodash'
  import i18n from '@/module/i18n'
  import mLocalParams from './_source/localParams'
  import mListBox from './_source/listBox'
  import mList4Box from './_source/list4Box'
  import Treeselect from '@riophae/vue-treeselect'
  import '@riophae/vue-treeselect/dist/vue-treeselect.css'
  import disabledState from '@/module/mixin/disabledState'
  import Clipboard from 'clipboard'
  import { diGuiTree, searchTree } from './_source/resourceTree'

  export default {
    name: 'spark',
    data () {
      return {
        valueConsistsOf: 'LEAF_PRIORITY',
        // Main function class
        mainClass: '',
        // Master jar package
        mainJar: null,
        // Master jar package(List)
        mainJarLists: [],
        mainJarList: [],
        // Deployment method
        deployMode: 'cluster',
        // Resource(list)
        resourceList: [],
        // Cache ResourceList
        cacheResourceList: [],
        // Custom function
        localParams: [],
        // Driver cores
        driverCores: 1,
        // Driver memory
        driverMemory: '512M',
        // Executor number
        numExecutors: 2,
        // Executor memory
        executorMemory: '2G',
        // Executor cores
        executorCores: 2,
        // Spark app name
        appName: '',
        // Main arguments
        mainArgs: '',
        // Option parameters
        others: '',
        // Program type
        programType: 'SCALA',
        // Program type(List)
        programTypeList: [{ code: 'JAVA' }, { code: 'SCALA' }, { code: 'PYTHON' }],
        // Spark version
        sparkVersion: 'SPARK2',
        // Spark version(LIst)
        sparkVersionList: [{ code: 'SPARK2' }, { code: 'SPARK1' }],
        normalizer (node) {
          return {
            label: node.name
          }
        },
        allNoResources: [],
        noRes: []
      }
    },
    props: {
      backfillItem: Object
    },
    mixins: [disabledState],
    methods: {
      _initRule () {
        this.rule = [
          {
            type: 'div',
            class: 'spark-model',
            children: [
              {
                type: 'm-list-box',
                field: 'type',
                children: [
                  {
                    type: 'div',
                    slot: 'text',
                    children: [i18n.$t('Program Type')]
                  },
                  {
                    type: 'div',
                    slot: 'content',
                    children: [
                      {
                        type: 'el-select',
                        style: 'width: 130px',
                        field: 'programType',
                        props: {
                          size: 'small',
                          disabled: this.isDetails
                        },
                        value: this.programTypeList,
                        options: this.programTypeList
                      }
                    ]
                  }
                ]
              },
              {
                type: 'm-list-box',
                field: 'version',
                children: [
                  {
                    type: 'div',
                    slot: 'text',
                    children: [i18n.$t('Spark Version')]
                  },
                  {
                    type: 'div',
                    slot: 'content',
                    children: [
                      {
                        type: 'el-select',
                        style: 'width: 130px',
                        field: 'sparkVersion',
                        props: {
                          size: 'small',
                          disabled: this.isDetails
                        },
                        value: this.sparkVersionList,
                        options: this.sparkVersionList
                      }
                    ]
                  }
                ]
              },
              this.programType !== 'PYTHON' ? {
                type: 'm-list-box',
                field: '_mainClass',
                children: [
                  {
                    type: 'div',
                    slot: 'text',
                    children: [i18n.$t('Main Class')]
                  },
                  {
                    type: 'div',
                    slot: 'content',
                    children: [
                      {
                        type: 'el-input',
                        field: 'mainClass',
                        props: {
                          type: 'input',
                          size: 'small',
                          disabled: this.isDetails,
                          placeholder: i18n.$t('Please enter main class')
                        }
                      }
                    ]
                  }
                ]
              } : undefined,
              {
                type: 'm-list-box',
                field: '_mainJar',
                native: true,
                children: [
                  {
                    type: 'div',
                    slot: 'text',
                    children: [i18n.$t('Main Jar Package')]
                  },
                  {
                    type: 'div',
                    slot: 'content',
                    children: [
                      {
                        type: 'treeselect',
                        field: 'mainJar',
                        native: true,
                        props: {
                          maxHeight: '200',
                          options: this.mainJarLists
                        },
                        sync: [{ options: this.mainJarLists }],
                        children: [
                          {
                            type: 'div',
                            slot: 'value-label',
                            field: 'node'
                          }
                        ]
                      }
                    ]
                  }
                ]
              },
              {
                type: 'm-list-box',
                field: '_deployMode',
                native: true,
                children: [
                  {
                    type: 'div',
                    slot: 'text',
                    children: [i18n.$t('Deploy Mode')]
                  },
                  {
                    type: 'div',
                    slot: 'content',
                    children: [
                      {
                        type: 'radio',
                        field: 'deployMode',
                        props: {
                          size: 'small'
                        },
                        options: [
                          {
                            value: 'cluster',
                            label: 'cluster',
                            disabled: this.isDetails
                          },
                          {
                            value: 'client',
                            label: 'client',
                            disabled: this.isDetails
                          },
                          {
                            value: 'local',
                            label: 'local',
                            disabled: this.isDetails
                          }
                        ]
                      }
                    ]
                  }
                ]
              },
              {
                type: 'm-list-box',
                field: '_appName',
                children: [
                  {
                    type: 'div',
                    slot: 'text',
                    children: [i18n.$t('App Name')]
                  },
                  {
                    type: 'div',
                    slot: 'content',
                    children: [
                      {
                        type: 'el-input',
                        field: 'appName',
                        props: {
                          type: 'input',
                          size: 'small',
                          disabled: this.isDetails,
                          placeholder: i18n.$t('Please enter app name(optional)')
                        }
                      }
                    ]
                  }
                ]
              },
              {
                type: 'm-list-4-box',
                field: '_driver',
                children: [
                  {
                    type: 'div',
                    slot: 'text',
                    children: [i18n.$t('Driver Cores')]
                  },
                  {
                    type: 'div',
                    slot: 'content',
                    children: [
                      {
                        type: 'el-input',
                        field: 'driverCores',
                        props: {
                          type: 'input',
                          size: 'small',
                          disabled: this.isDetails,
                          placeholder: i18n.$t('Please enter Driver cores')
                        }
                      }
                    ]
                  },
                  {
                    type: 'div',
                    slot: 'text-2',
                    children: [i18n.$t('Driver Memory')]
                  },
                  {
                    type: 'div',
                    slot: 'content-2',
                    children: [
                      {
                        type: 'el-input',
                        field: 'driverMemory',
                        props: {
                          type: 'input',
                          size: 'small',
                          disabled: this.isDetails,
                          placeholder: i18n.$t('Please enter Driver memory')
                        }
                      }
                    ]
                  }
                ]
              },
              {
                type: 'm-list-4-box',
                field: '_executor',
                children: [
                  {
                    type: 'div',
                    slot: 'text',
                    children: [i18n.$t('Executor Number')]
                  },
                  {
                    type: 'div',
                    slot: 'content',
                    children: [
                      {
                        type: 'el-input',
                        field: 'numExecutors',
                        props: {
                          type: 'input',
                          size: 'small',
                          disabled: this.isDetails,
                          placeholder: i18n.$t('Please enter Executor number')
                        }
                      }
                    ]
                  },
                  {
                    type: 'div',
                    slot: 'text-2',
                    children: [i18n.$t('Executor Memory')]
                  },
                  {
                    type: 'div',
                    slot: 'content-2',
                    children: [
                      {
                        type: 'el-input',
                        field: 'executorMemory',
                        props: {
                          type: 'input',
                          size: 'small',
                          disabled: this.isDetails,
                          placeholder: i18n.$t('Please enter Executor memory')
                        }
                      }
                    ]
                  }
                ]
              },
              {
                type: 'm-list-4-box',
                field: '_executor2',
                children: [
                  {
                    type: 'div',
                    slot: 'text',
                    children: [i18n.$t('Executor Cores')]
                  },
                  {
                    type: 'div',
                    slot: 'content',
                    children: [
                      {
                        type: 'el-input',
                        field: 'executorCores',
                        props: {
                          type: 'input',
                          size: 'small',
                          disabled: this.isDetails,
                          placeholder: i18n.$t('Please enter Executor cores')
                        }
                      }
                    ]
                  }
                ]
              },
              {
                type: 'm-list-box',
                field: '_mainArgs',
                children: [
                  {
                    type: 'div',
                    slot: 'text',
                    children: [i18n.$t('Main Arguments')]
                  },
                  {
                    type: 'div',
                    slot: 'content',
                    children: [
                      {
                        type: 'el-input',
                        field: 'mainArgs',
                        props: {
                          type: 'textarea',
                          size: 'small',
                          autosize: { minRows: 2 },
                          disabled: this.isDetails,
                          placeholder: i18n.$t('Please enter main arguments')
                        }
                      }
                    ]
                  }
                ]
              },
              {
                type: 'm-list-box',
                field: '_others',
                children: [
                  {
                    type: 'div',
                    slot: 'text',
                    children: [i18n.$t('Option Parameters')]
                  },
                  {
                    type: 'div',
                    slot: 'content',
                    children: [
                      {
                        type: 'el-input',
                        field: 'others',
                        props: {
                          type: 'textarea',
                          size: 'small',
                          autosize: { minRows: 2 },
                          disabled: this.isDetails,
                          placeholder: i18n.$t('Please enter option parameters')
                        }
                      }
                    ]
                  }
                ]
              },
              {
                type: 'm-list-box',
                field: '_resourceList',
                native: true,
                children: [
                  {
                    type: 'div',
                    slot: 'text',
                    children: [i18n.$t('Resources')]
                  },
                  {
                    type: 'div',
                    slot: 'content',
                    children: [
                      {
                        type: 'treeselect',
                        field: 'resourceList',
                        native: true,
                        props: {
                          multiple: true,
                          maxHeight: '200',
                          options: this.mainJarList,
                          normalizer: this.normalizer,
                          valueConsistsOf: this.valueConsistsOf,
                          disabled: this.isDetails,
                          placeholder: i18n.$t('Please select resources')
                        },
                        sync: [
                          { options: this.mainJarLists },
                          { valueConsistsOf: this.valueConsistsOf },
                          { disabled: this.isDetails }
                        ],
                        children: [
                          {
                            type: 'div',
                            slot: 'value-label',
                            field: 'node',
                            children: [
                              {
                                type: 'span',
                                class: 'copy-path',
                                on: {
                                  mousedown: ($event, node) =>
                                    this._copyPath($event, node)
                                },
                                children: [
                                  {
                                    type: 'em',
                                    class: 'el-icon-copy-document',
                                    title: i18n.$t('Copy path'),
                                    props: {
                                      dataContainer: 'body',
                                      dataToggle: 'tooltip'
                                    }
                                  }
                                ]
                              }
                            ]
                          }
                        ]
                      }
                    ]
                  }
                ]
              },
              {
                type: 'm-list-box',
                field: 'cusParams',
                native: true,
                children: [
                  {
                    type: 'div',
                    slot: 'text',
                    children: [i18n.$t('Custom Parameters')]
                  },
                  {
                    type: 'div',
                    slot: 'content',
                    children: [
                      {
                        type: 'm-local-params',
                        field: 'refLocalParams',
                        props: {
                          udpList: this.localParams,
                          hide: false
                        },
                        sync: [{ udpList: this.localParams }],
                        on: {
                          'on-local-params': this._onLocalParams
                        }
                      }
                    ]
                  }
                ]
              }
            ]
          }
        ]
      },
      _copyPath (e, node) {
        e.stopPropagation()
        let clipboard = new Clipboard('.copy-path', {
          text: function () {
            return node.raw.fullName
          }
        })
        clipboard.on('success', handler => {
          this.$message.success(`${i18n.$t('Copy success')}`)
          // Free memory
          clipboard.destroy()
        })
        clipboard.on('error', handler => {
          // Copy is not supported
          this.$message.warning(`${i18n.$t('The browser does not support automatic copying')}`)
          // Free memory
          clipboard.destroy()
        })
      },
      /**
       * getResourceId
       */
      marjarId (name) {
        this.store.dispatch('dag/getResourceId', {
          type: 'FILE',
          fullName: '/' + name
        }).then(res => {
          this.mainJar = res.id
        }).catch(e => {
          this.$message.error(e.msg || '')
        })
      },
      /**
       * return localParams
       */
      _onLocalParams (a) {
        this.localParams = a
      },
      /**
       * return resourceList
       */
      _onResourcesData (a) {
        this.resourceList = a
      },
      /**
       * cache resourceList
       */
      _onCacheResourcesData (a) {
        this.cacheResourceList = a
      },
      dataProcess (backResource) {
        let isResourceId = []
        let resourceIdArr = []
        if (this.resourceList.length > 0) {
          this.resourceList.forEach(v => {
            this.mainJarList.forEach(v1 => {
              if (searchTree(v1, v)) {
                isResourceId.push(searchTree(v1, v))
              }
            })
          })
          resourceIdArr = isResourceId.map(item => {
            return item.id
          })
          Array.prototype.diff = function (a) {
            return this.filter(function (i) { return a.indexOf(i) < 0 })
          }
          let diffSet = this.resourceList.diff(resourceIdArr)
          let optionsCmp = []
          if (diffSet.length > 0) {
            diffSet.forEach(item => {
              backResource.forEach(item1 => {
                if (item === item1.id || item === item1.res) {
                  optionsCmp.push(item1)
                }
              })
            })
          }
          let noResources = [{
            id: -1,
            name: $t('Unauthorized or deleted resources'),
            fullName: '/' + $t('Unauthorized or deleted resources'),
            children: []
          }]
          if (optionsCmp.length > 0) {
            this.allNoResources = optionsCmp
            optionsCmp = optionsCmp.map(item => {
              return { id: item.id, name: item.name, fullName: item.res }
            })
            optionsCmp.forEach(item => {
              item.isNew = true
            })
            noResources[0].children = optionsCmp
            this.mainJarList = this.mainJarList.concat(noResources)
          }
        }
      },
      /**
       * verification
       */
      _verification () {
        if (this.programType !== 'PYTHON' && !this.mainClass) {
          this.$message.warning(`${i18n.$t('Please enter main class')}`)
          return false
        }

        if (!this.mainJar) {
          this.$message.warning(`${i18n.$t('Please enter main jar package')}`)
          return false
        }

        if (!this.driverCores) {
          this.$message.warning(`${i18n.$t('Please enter Driver cores')}`)
          return false
        }

        if (!Number.isInteger(parseInt(this.driverCores))) {
          this.$message.warning(`${i18n.$t('Core number should be positive integer')}`)
          return false
        }

        if (!this.driverMemory) {
          this.$message.warning(`${i18n.$t('Please enter Driver memory')}`)
          return false
        }

        if (!Number.isInteger(parseInt(this.driverMemory))) {
          this.$message.warning(`${i18n.$t('Memory should be a positive integer')}`)
          return false
        }

        if (!this.executorCores) {
          this.$message.warning(`${i18n.$t('Please enter Executor cores')}`)
          return false
        }

        if (!Number.isInteger(parseInt(this.executorCores))) {
          this.$message.warning(`${i18n.$t('Core number should be positive integer')}`)
          return false
        }

        if (!this.executorMemory) {
          this.$message.warning(`${i18n.$t('Please enter Executor memory')}`)
          return false
        }

        if (!Number.isInteger(parseInt(this.executorMemory))) {
          this.$message.warning(`${i18n.$t('Memory should be a positive integer')}`)
          return false
        }

        if (!this.numExecutors) {
          this.$message.warning(`${i18n.$t('Please enter Executor number')}`)
          return false
        }

        if (!Number.isInteger(parseInt(this.numExecutors))) {
          this.$message.warning(`${i18n.$t('The Executor number should be a positive integer')}`)
          return false
        }

        // noRes
        if (this.noRes.length > 0) {
          this.$message.warning(`${i18n.$t('Please delete all non-existent resources')}`)
          return false
        }

        // localParams Subcomponent verification
        if (!this.$refs.refLocalParams._verifProp()) {
          return false
        }
        // Process resourcelist
        let dataProcessing = _.map(this.resourceList, v => {
          return {
            id: v
          }
        })

        // storage
        this.$emit('on-params', {
          mainClass: this.mainClass,
          mainJar: {
            id: this.mainJar
          },
          deployMode: this.deployMode,
          resourceList: dataProcessing,
          localParams: this.localParams,
          driverCores: this.driverCores,
          driverMemory: this.driverMemory,
          numExecutors: this.numExecutors,
          executorMemory: this.executorMemory,
          executorCores: this.executorCores,
          appName: this.appName,
          mainArgs: this.mainArgs,
          others: this.others,
          programType: this.programType,
          sparkVersion: this.sparkVersion
        })
        return true
      }
    },
    watch: {
      // Listening type
      programType (type) {
        if (type === 'PYTHON') {
          this.mainClass = ''
        }
      },
      // Watch the cacheParams
      cacheParams (val) {
        this.$emit('on-cache-params', val)
      },
      resourceIdArr (arr) {
        let result = []
        arr.forEach(item => {
          this.allNoResources.forEach(item1 => {
            if (item.id === item1.id) {
              // resultBool = true
              result.push(item1)
            }
          })
        })
        this.noRes = result
      }
    },
    computed: {
      resourceIdArr () {
        let isResourceId = []
        let resourceIdArr = []
        if (this.resourceList.length > 0) {
          this.resourceList.forEach(v => {
            this.mainJarList.forEach(v1 => {
              if (searchTree(v1, v)) {
                isResourceId.push(searchTree(v1, v))
              }
            })
          })
          resourceIdArr = isResourceId.map(item => {
            return { id: item.id, name: item.name, res: item.fullName }
          })
        }
        return resourceIdArr
      },
      cacheParams () {
        return {
          mainClass: this.mainClass,
          mainJar: {
            id: this.mainJar
          },
          deployMode: this.deployMode,
          resourceList: this.resourceIdArr,
          localParams: this.localParams,
          driverCores: this.driverCores,
          driverMemory: this.driverMemory,
          numExecutors: this.numExecutors,
          executorMemory: this.executorMemory,
          executorCores: this.executorCores,
          appName: this.appName,
          mainArgs: this.mainArgs,
          others: this.others,
          programType: this.programType,
          sparkVersion: this.sparkVersion
        }
      }
    },
    created () {
      let item = this.store.state.dag.resourcesListS
      let items = this.store.state.dag.resourcesListJar
      diGuiTree(item)
      diGuiTree(items)
      this.mainJarList = item
      this.mainJarLists = items
      let o = this.backfillItem

      // Non-null objects represent backfill
      if (!_.isEmpty(o)) {
        this.mainClass = o.params.mainClass || ''
        if (o.params.mainJar.res) {
          this.marjarId(o.params.mainJar.res)
        } else if (o.params.mainJar.res === '') {
          this.mainJar = ''
        } else {
          this.mainJar = o.params.mainJar.id || ''
        }
        this.deployMode = o.params.deployMode || ''
        this.driverCores = o.params.driverCores || 1
        this.driverMemory = o.params.driverMemory || '512M'
        this.numExecutors = o.params.numExecutors || 2
        this.executorMemory = o.params.executorMemory || '2G'
        this.executorCores = o.params.executorCores || 2
        this.appName = o.params.appName || ''
        this.mainArgs = o.params.mainArgs || ''
        this.others = o.params.others
        this.programType = o.params.programType || 'SCALA'
        this.sparkVersion = o.params.sparkVersion || 'SPARK2'

        // backfill resourceList
        let backResource = o.params.resourceList || []
        let resourceList = o.params.resourceList || []
        if (resourceList.length) {
          _.map(resourceList, v => {
            if (!v.id) {
              this.store.dispatch('dag/getResourceId', {
                type: 'FILE',
                fullName: '/' + v.res
              }).then(res => {
                this.resourceList.push(res.id)
                this.dataProcess(backResource)
              }).catch(e => {
                this.resourceList.push(v.res)
                this.dataProcess(backResource)
              })
            } else {
              this.resourceList.push(v.id)
              this.dataProcess(backResource)
            }
          })
          this.cacheResourceList = resourceList
        }

        // backfill localParams
        let localParams = o.params.localParams || []
        if (localParams.length) {
          this.localParams = localParams
        }
      }
    },
    mounted () {

    },
    components: { mLocalParams, mListBox, mList4Box, Treeselect }
  }
</script>
