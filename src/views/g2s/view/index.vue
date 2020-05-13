<template>
  <div class="mainContent">
    <div style="position:relative">
      <h1 class="title">{{ this.g2s.name }}</h1>
      <el-button
        @click="folk()"
        style="position:absolute; right:65px;top:0px;"
        type="info"
        circle
        icon="el-icon-connection"
      >
      </el-button>
    </div>
    <el-row>
      <el-col :span="22" :offset="1">
        <el-row>
          <el-col :span="5">
            <el-card>
              <div class="single-info">
                <div class="info-tag">
                  <i class="el-icon-user-solid" />
                  <strong>Creator</strong>
                </div>
                <span>{{ this.g2s.creator }}</span>
              </div>

              <div class="single-info">
                <div class="info-tag">
                  <i class="el-icon-time" />
                  <strong>Create Time</strong>
                </div>
                <span>{{ this.g2s.createTime }}</span>
              </div>

              <el-collapse v-model="activeNamesLeft">
                <el-collapse-item title="goals" name="1">
                  <div>{{ g2s.goals }}</div>
                </el-collapse-item>
                <el-collapse-item title="background" name="2">
                  <div>{{ g2s.background }}</div>
                </el-collapse-item>
              </el-collapse>
            </el-card>
          </el-col>
          <el-col :span="18" :offset="1">
            <div class="rightContent">
              <el-card>
                <el-collapse v-model="activeNamesRight">
                  <el-collapse-item title="Context Define" name="1">
                    <el-tabs v-model="activeContext">
                      <el-tab-pane
                        label="Theme"
                        name="theme"
                        v-if="g2s.contextDefine"
                      >
                        {{ g2s.contextDefine.theme }}
                      </el-tab-pane>
                      <el-tab-pane label="Geographic Object" name="object">
                        {{ g2s.contextDefine.object }}
                      </el-tab-pane>
                      <el-tab-pane label="Boundary" name="boundary">
                        {{ g2s.contextDefine.boundary }}
                      </el-tab-pane>
                    </el-tabs>
                  </el-collapse-item>
                  <el-collapse-item title="Resource Collect" name="2">
                    <el-tabs v-model="activeResource">
                      <el-tab-pane label="Data Services" name="data">
                        <el-table :data="dataTable" style="width: 100%">
                          <el-table-column prop="name" label="Name" width="180">
                          </el-table-column>
                          <el-table-column
                            prop="createTime"
                            label="CreateTime"
                            width="180"
                          >
                          </el-table-column>
                          <el-table-column
                            prop="description"
                            label="description"
                          >
                          </el-table-column>
                          <el-table-column
                            fixed="right"
                            label="operation"
                            width="120"
                          >
                            <template slot-scope="scope">
                              <el-button
                                @click.native.prevent="download(scope.row)"
                                type="text"
                                size="small"
                              >
                                download
                              </el-button>
                            </template>
                          </el-table-column>
                        </el-table>
                      </el-tab-pane>

                      <el-tab-pane label="Data Process Services" name="process">
                        <el-table :data="dataProcessTable" style="width: 100%">
                          <el-table-column prop="name" label="Name" width="180">
                          </el-table-column>
                          <el-table-column
                            prop="createTime"
                            label="CreateTime"
                            width="180"
                          >
                          </el-table-column>
                          <el-table-column
                            prop="description"
                            label="description"
                          >
                          </el-table-column>
                          <el-table-column
                            fixed="right"
                            label="operation"
                            width="120"
                          >
                            <template slot-scope="scope">
                              <el-button
                                @click.native.prevent="
                                  view(scope.row, 'process')
                                "
                                type="text"
                                size="small"
                              >
                                view
                              </el-button>

                              <el-button
                                @click.native.prevent="
                                  invoke(scope.row, 'process')
                                "
                                type="text"
                                size="small"
                              >
                                invoke
                              </el-button>
                            </template>
                          </el-table-column>
                        </el-table>
                      </el-tab-pane>

                      <el-tab-pane label="Model Services" name="model">
                        <el-table :data="modelTable" style="width: 100%">
                          <el-table-column prop="name" label="Name" width="180">
                          </el-table-column>
                          <el-table-column
                            prop="createTime"
                            label="CreateTime"
                            width="180"
                          >
                          </el-table-column>
                          <el-table-column
                            prop="description"
                            label="description"
                          >
                          </el-table-column>

                          <el-table-column
                            fixed="right"
                            label="operation"
                            width="120"
                          >
                            <template slot-scope="scope">
                              <el-button
                                @click.native.prevent="view(scope.row, 'model')"
                                type="text"
                                size="small"
                              >
                                view
                              </el-button>

                              <el-button
                                @click.native.prevent="
                                  invoke(scope.row, 'model')
                                "
                                type="text"
                                size="small"
                              >
                                invoke
                              </el-button>
                            </template>
                          </el-table-column>
                        </el-table>
                      </el-tab-pane>
                    </el-tabs>
                  </el-collapse-item>
                </el-collapse>
              </el-card>

              <el-card>
                <h1>Simulation Concept Graph</h1>
                <div
                  style="width:90%;margin-top:20px;padding:15px;background-color:#f8f8f9"
                  ref="mx"
                ></div>
              </el-card>

              <el-card>
                <el-tabs v-model="activeExpected">
                  <el-tab-pane label="Service Instances" name="instance">
                    <InstanceCard
                      v-for="(instance, index) in instanceCard"
                      :key="index"
                      :cardData="instance"
                    ></InstanceCard>
                  </el-tab-pane>

                  <el-tab-pane label="Evaluation" name="evaluation">
                    <InstanceCard
                      v-for="(evaluation, index) in evaluationCard"
                      :key="index"
                      :cardData="evaluation"
                    ></InstanceCard>
                  </el-tab-pane>

                  <el-tab-pane label="Workflow" name="workflow">
                    <el-button
                      @click="fullScreen"
                      icon="el-icon-full-screen"
                      primary
                    ></el-button>
                    <workflow
                      id="fullScreenComponent"
                      :g2sId="this.id"
                    ></workflow>
                  </el-tab-pane>
                </el-tabs>
              </el-card>
            </div>
          </el-col>
        </el-row>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import mxgraph from "@/lib/mx/index.js";
import workflow from "./components/workflow";
const { mxGraph, mxClient, mxCodec, mxUtils } = mxgraph;
import { get } from "@/axios";
import InstanceCard from "_com/common/InstanceCard";
export default {
  data() {
    return {
      id: this.$route.params.id,
      g2s: {
        contextDefine: {
          theme: {},
          object: {},
          boundary: {}
        },
        resourceCollect: {
          dataServices: [],
          modelServices: [],
          dataProcessServices: []
        },
        computation: {
          serviceInstances: []
        },
        evaluation: []
      },
      activeNamesLeft: ["1", "2"],
      activeNamesRight: ["1", "2"],
      activeResource: "data",
      activeContext: "theme",
      activeExpected: "instance",
      fullscreenFlag: false
    };
  },
  computed: {
    instanceCard() {
      let arr = [];
      this.g2s.computation.serviceInstances.forEach(
        ({
          id,
          name,
          description,
          createTime,
          creator,
          instanceEnum,
          service
        }) => {
          let inner = {
            id,
            name,
            description,
            createTime,
            creator,
            serviceId: service.id
          };
          if (instanceEnum === "MODEL") {
            inner.type = "model";
          } else {
            inner.type = "process";
          }
          arr.push(inner);
        }
      );
      return arr;
    },
    evaluationCard() {
      let arr = [];
      this.g2s.evaluation.forEach(
        ({ id, name, description, createTime, creator }) => {
          let inner = {
            id,
            name,
            description,
            createTime,
            creator
          };
          inner.type = "evaluation";
          arr.push(inner);
        }
      );
      return arr;
    },
    dataTable() {
      let arr = [];
      this.g2s.resourceCollect.dataServices.forEach(
        ({ id, name, description, createTime }) => {
          let inner = {
            id,
            name,
            description,
            createTime
          };
          arr.push(inner);
        }
      );
      return arr;
    },
    dataProcessTable() {
      let arr = [];
      this.g2s.resourceCollect.dataProcessServices.forEach(
        ({ id, name, description, createTime }) => {
          let inner = {
            id,
            name,
            description,
            createTime
          };
          arr.push(inner);
        }
      );
      return arr;
    },
    modelTable() {
      let arr = [];
      this.g2s.resourceCollect.modelServices.forEach(
        ({ id, name, description, createTime }) => {
          let inner = {
            id,
            name,
            description,
            createTime
          };
          arr.push(inner);
        }
      );
      return arr;
    }
  },

  methods: {
    download(row) {
      window.open(`http://localhost:8081/data_service/fetch/${row.id}`);
    },
    view(row, type) {
      this.$router.push({
        path: `/resource/${row.id}/${type}`
      });
    },
    invoke(row, type) {
      this.$router.push({
        path: `/resource/${row.id}/${type}/invoke`
      });
    },
    fullScreen() {
      let element = document.getElementById("fullScreenComponent");
      element.requestFullscreen();
      this.fullscreenFlag = true;
    },
    folk() {
      console.log("todo" + this.g2s.id);
    },
    initMxgraph() {
      if (!mxClient.isBrowserSupported()) {
        // 判断是否支持mxgraph
        mxUtils.error("Browser is not supported!", 200, false);
      } else {
        // 再容器中创建图表
        let container = this.$refs.mx;

        let graph = new mxGraph(container);
        graph.setEnabled(false);

        graph.getModel().beginUpdate();
        try {
          let xmlData = this.g2s.simulationConceptGraph.xmlGraph;
          let xmlDoc = mxUtils.parseXml(xmlData);
          let codec = new mxCodec(xmlDoc);
          codec.decode(xmlDoc.documentElement, graph.getModel());
        } finally {
          // Updates the display
          graph.getModel().endUpdate();
        }
      }
    }
  },
  created() {
    document.addEventListener("keyup", el => {
      if (el.keyCode == 27) {
        this.fullscreenFlag = false;
      }
    });
  },
  async mounted() {
    this.g2s = await get("/g2s/{id}/view", null, {
      id: this.id
    });
    this.initMxgraph();
  },
  components: {
    workflow,
    InstanceCard
  }
};
</script>

<style scoped>
.mainContent {
  /* background-color: rgba(116, 111, 111, 0.2); */
  height: 700px;
}
.title {
  text-align: center;
  margin-top: 30px;
  margin-bottom: 20px;
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
  color: #0366d6;
}
.info {
  display: flex;
  align-items: center;
  margin-right: 10px;
}
.single-info {
  display: flex;
  align-items: flex-start;
  padding: 5px;
  /* height: 30px; */
  font-size: 12px;
  line-height: 15px;
}
.info-tag {
  display: flex;
  width: 120px;
  align-items: center;
}
.rightContent {
  /* margin-top: 20px; */
  flex: 1;
}
</style>
