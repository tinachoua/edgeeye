<template>
  <q-page class="q-pa-sm">
    <div class="row q-col-gutter-md">
      <!-- <div class="col-lg-3 col-md-3 col-xs-12 col-sm-12"> -->
      <div class="col-lg-3 col-md-3 col-xs-12 col-sm-12">
        <q-card class="bg-cyan-10 text-white" style="height: 91vh">
          <q-card-section class="text-center bg-transparent">
            <q-avatar size="100px" class="shadow-10">
              <!-- <img
                style="background-color: white"
                src="../assets/baseline_desktop_windows_black_24dp.png"

              /> -->
              <q-img src="../assets/computer.png"  height="100px" fit="fill" > </q-img>
            </q-avatar>
            <div class="text-subtitle2 q-mt-lg" @click="isAIHide = !isAIHide">
              <q-chip
                v-if="devStatus == 'online'"
                color="green-3"
                text-color="black"
              >
                {{ devName }}
              </q-chip>
              <q-chip v-else color="red-13" text-color="white">
                {{ devName }}
              </q-chip>
            </div>
            <div class="text-subtitle2 q-mt-lg">
              {{ devAlias }}
            </div>
            <!-- @click="toggleAI()" <i class="fa-solid fa-power-off"></i>-->
            <div class="text-h6 q-mt-md">Jetson series</div>
            <div class="text-h6 q-mt-md">{{ devIp }}</div>
            <div class="row text-h6 q-mt-md">
              <div class="col">
                <q-btn
                  style="background: #5064b5; color: white"
                  icon="restart_alt"
                  label="Reboot"
                  disable
                />
              </div>
              <!-- <div class="col">
                <q-btn
                  style="background: #5064b5; color: white"
                  label="Recovery"
                  disable
                />
              </div> -->
              <div class="col">
                <q-btn
                  style="background: #5064b5; color: white"
                  label="Shutdown"
                  disable
                />
              </div>
            </div>
            <br />
            <hr />
          </q-card-section>
          <q-card-section>
            <div class="col q-col-gutter-md">
              <div
                v-for="(item, index) in overviewArr"
                :key="index"
                class="col-md-3 col-sm-12 col-xs-12"
              >
                <div class="text-body2 text-justify">{{ index }}</div>
                <div class="text-body1 text-justify text-weight-bold">
                  {{ item }}
                </div>
              </div>
            </div>

            <div class="text-body2 text-justify"></div>
          </q-card-section>
        </q-card>
      </div>
      <div class="col-lg-9 col-md-9 col-xs-12 col-sm-12 q-row-gutter-md">
        <q-card class="bg-cyan-10 text-white">
          <q-tabs
            v-model="tab"
            dense
            class="bg-cyan-10 text-teal-3"
            active-color="lime-2"
            indicator-color="lime-2"
            align="justify"
          >
            <q-tab name="mails" label="Version" />
            <q-tab name="alarms" label="Hardware" />
          </q-tabs>

          <q-tab-panels
            v-model="tab"
            animated
            class="bg-blue-grey-1 text-black"
            style="min-height: 322px; height: 86.7vh"
          >
            <q-tab-panel name="mails">
              <span
                v-if="osArr.length != 0"
                class="text-h6 q-mr-sm q-mb-sm q-pl-md"
                style="
                  width: auto;
                  border-left: 5px solid green;
                  background: rgba(220, 220, 220, 0.3);
                "
                >Operating System Information</span
              >
              <q-card class="bg-transparent no-shadow no-border">
                <q-card-section class="q-pa-none">
                  <div class="row q-col-gutter-md">
                    <div
                      v-for="(item, index) in osArr"
                      :key="index"
                      class="col-md-3 col-sm-12 col-xs-12"
                    >
                      <q-item
                        :style="`background-color: #7cb342`"
                        class="q-pa-none"
                      >
                        <q-item-section class="q-pa-md q-ml-none text-white">
                          <q-item-label>{{ index }}</q-item-label>
                          <q-item-label
                            class="text-white text-body1 text-weight-bolder"
                            >{{ item }}</q-item-label
                          >
                        </q-item-section>
                      </q-item>
                    </div>
                  </div>
                </q-card-section>
              </q-card>

              <br />
              <span
                v-if="libArr.length != 0"
                class="text-h6 q-mr-sm q-mb-sm q-pl-md"
                style="
                  width: auto;
                  border-left: 5px solid green;
                  background: rgba(220, 220, 220, 0.3);
                "
                >Library Version</span
              >

              <q-card class="bg-transparent no-shadow no-border">
                <q-card-section class="q-pa-none">
                  <div class="row q-col-gutter-md">
                    <div
                      v-for="(item, index) in libArr"
                      :key="index"
                      class="col-md-3 col-sm-12 col-xs-12"
                    >
                      <q-item
                        :style="`background-color: #7cb342`"
                        class="q-pa-none"
                      >
                        <q-item-section class="q-pa-md q-ml-none text-white">
                          <q-item-label>{{ index }}</q-item-label>
                          <q-item-label
                            class="text-white text-body1 text-weight-bolder"
                            >{{ item }}</q-item-label
                          >
                        </q-item-section>
                      </q-item>
                    </div>
                  </div>
                </q-card-section>
              </q-card>

              <br />

              <!-- <q-card class="bg-transparent no-shadow no-border">
                <q-card-section class="q-pa-none">
                  <div class="row q-col-gutter-md">
                    <div
                      v-for="(item, index) in items"
                      :key="index"
                      class="col-md-3 col-sm-12 col-xs-12"
                    >
                      <q-item
                        :style="`background-color: ${item.color1}`"
                        class="q-pa-none"
                      >
                        <q-item-section
                          v-if="icon_position === 'right'"
                          side
                          :style="`background-color: ${item.color2}`"
                          class="q-pa-lg q-mr-none text-white"
                        >
                          <q-icon
                            :name="item.icon"
                            color="white"
                            size="24px"
                          ></q-icon>
                        </q-item-section>
                        <q-item-section class="q-pa-md q-ml-none text-white">
                          <q-item-label>{{ item.title }}</q-item-label>
                          <q-item-label
                            class="text-white text-body1 text-weight-bolder"
                            >{{ item.value }}</q-item-label
                          >
                        </q-item-section>
                      </q-item>
                    </div>
                  </div>
                </q-card-section>
              </q-card> -->
            </q-tab-panel>

            <q-tab-panel name="alarms">
              <!-- <div v-for="(value, name, index) in cpuDynamic.CPU" :key="index">
                <div v-if="value.Freq">Core {{ index }}</div>
                {{ value.Freq }}
              </div> -->
<span
                v-if="cpuInfoArr.length != 0"
                class="text-h6 q-mr-sm q-mb-sm q-pl-md"
                style="
                  width: auto;
                  border-left: 5px solid green;
                  background: rgba(220, 220, 220, 0.3);
                "
                >CPU Information</span
              >

              <q-card class="bg-transparent no-shadow no-border">
                <q-card-section class="q-pa-none">
                  <div class="row q-col-gutter-md">
                    <div
                      v-for="(item, index) in cpuInfoArr"
                      :key="index"
                      class="col-md-3 col-sm-12 col-xs-12"
                    >
                      <q-item
                        :style="`background-color: #7cb342`"
                        class="q-pa-none"
                      >
                        <q-item-section class="q-pa-md q-ml-none text-white">
                          <q-item-label>{{ index }}</q-item-label>
                          <q-item-label
                            class="text-white text-body1 text-weight-bolder"
                            >{{ item }}</q-item-label
                          >
                        </q-item-section>
                      </q-item>
                    </div>
                  </div>
                </q-card-section>
              </q-card>

              <br />
              <span
                v-if="cpuDynamic.CPU.length != 0"
                class="text-h6 q-mr-sm q-mb-sm q-pl-md"
                style="
                  width: auto;
                  border-left: 5px solid green;
                  background: rgba(220, 220, 220, 0.3);
                "
                >CPU Core Info - {{ model }}
                <q-select
                  filled
                  v-model="model"
                  :options="options"
                  label="Filter Info"
              /></span>

              <q-card class="bg-transparent no-shadow no-border">
                <q-card-section class="q-pa-none">
                  <div class="row q-col-gutter-md">
                    <div
                      v-for="(item, index) in cpuDynamic.CPU"
                      :key="index"
                      class="col-md-3 col-sm-12 col-xs-12"
                    >
                      <q-item
                        v-if="item.Freq"
                        :style="`background-color: #7cb342`"
                        class="q-pa-none"
                      >
   
                      
                        <q-item-section class="q-pa-md q-ml-none text-white">
                          <q-item-label>Core {{ index }} </q-item-label>
                          <!-- <q-item-label
                            >Core {{ index }} - {{ model }}</q-item-label
                          > -->
                          <q-item-label
                            v-if="model == 'Frequency'"
                            class="text-white text-body1 text-weight-bolder"
                            >{{ item.Freq }} MHz</q-item-label
                          >
                          <q-item-label
                            v-if="model == 'Temperature'"
                            class="text-white text-body1 text-weight-bolder"
                            >{{ item.Temp }} ℃</q-item-label
                          >
                          <q-item-label
                            v-if="model == 'Usage'"
                            class="text-white text-body1 text-weight-bolder"
                            >{{ item.Usage.toFixed(2) }} %</q-item-label
                          >
                          <q-item-label
                            v-if="model == 'Voltage'"
                            class="text-white text-body1 text-weight-bolder"
                            >{{ item.V.toFixed(2) }} V</q-item-label
                          >
                        </q-item-section>
                      </q-item>
                    </div>
                  </div>
                </q-card-section>
              </q-card>

              <!-- <div
                v-for="(item, index) in newChartDataArr"
                :key="index"
                class="col-lg-6 col-md-6 col-sm-12 col-xs-12"
              > -->
              <div
                v-for="(item, index) in newChartDataArr"
                :key="index"
                class="col-lg-6 col-md-6 col-sm-12 col-xs-12"
              >
                <!-- <span
                  class="text-h6 q-mr-sm q-mb-sm q-pl-md"
                  style="
                    width: auto;
                    border-left: 5px solid green;
                    background: rgba(220, 220, 220, 0.3);
                  "
                  >{{ item.label }}</span
                > -->
                <line-chart-v :chart-data="item" :height="100"></line-chart-v>
                <br />
              </div>

              <!-- <span
                v-if="jetsonInfoArr.length != 0"
                class="text-h6 q-mr-sm q-mb-sm q-pl-md"
                style="
                  width: auto;
                  border-left: 5px solid green;
                  background: rgba(220, 220, 220, 0.3);
                "
                >Jetson stats</span
              >
              <q-card class="bg-transparent no-shadow no-border">
                <q-card-section class="q-pa-none">
                  <div class="row q-col-gutter-md">
                    <div
                      v-for="(item, index) in jetsonInfoArr"
                      :key="index"
                      class="col-md-3 col-sm-12 col-xs-12"
                    >
                      <q-item
                        :style="`background-color: #7cb342`"
                        class="q-pa-none"
                      >
                        <q-item-section
                          v-if="icon_position === 'right'"
                          side
                          :style="`background-color: #7cb342`"
                          class="q-pa-lg q-mr-none text-white"
                        >
                          <q-icon
                            :name="item.icon"
                            color="white"
                            size="24px"
                          ></q-icon>
                        </q-item-section>
                        <q-item-section class="q-pa-md q-ml-none text-white">
                          <q-item-label>{{ item.title }}</q-item-label>
                          <q-item-label
                            class="text-white text-body1 text-weight-bolder"
                            >{{ item.value }}</q-item-label
                          >
                        </q-item-section>
                      </q-item>
                    </div>
                  </div>
                </q-card-section>
              </q-card>
              <br /> -->
              <!-- <span
                class="text-h6 q-mr-sm q-mb-sm q-pl-md"
                style="
                  width: auto;
                  border-left: 5px solid green;
                  background: rgba(220, 220, 220, 0.3);
                "
                >CPU</span
              >
              <br />
              <div class="row q-gutter-md">
                <div class="col">
                  <span> Fan </span>
                  <q-field outlined bg-color="white" :dense="dense">
                    <template v-slot:prepend>
                      <div
                        class="self-center full-width no-outline"
                        tabindex="0"
                      >
                        160 RPM
                      </div>
                    </template>

                    <template v-slot:append>
                      <q-avatar>
                        <img
                          src="https://pic.onlinewebfonts.com/svg/img_489522.png"
                        />
                      </q-avatar>
                    </template>
                  </q-field>
                </div>
                <div class="col">
                  <span> Frequency </span>
                  <q-field outlined bg-color="white" stack-label :dense="dense">
                    <template v-slot:prepend>
                      <div
                        class="self-center full-width no-outline"
                        tabindex="0"
                      >
                        1907 MHz
                      </div>
                    </template>

                    <template v-slot:append>
                      <q-avatar>
                        <img
                          src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSXgw0imKP2k_AqSMDHgRhImrTFXcKISeqWlg&usqp=CAU"
                        />
                      </q-avatar>
                    </template>
                  </q-field>
                </div>
                <div class="col"></div>
                <div class="col"></div>
              </div> -->
              <br />

              <span
                v-if="mbInfoArr.length != 0"
                class="text-h6 q-mr-sm q-mb-sm q-pl-md"
                style="
                  width: auto;
                  border-left: 5px solid green;
                  background: rgba(220, 220, 220, 0.3);
                "
                >Motherboard Information</span
              >

              <q-card class="bg-transparent no-shadow no-border">
                <q-card-section class="q-pa-none">
                  <div class="row q-col-gutter-md">
                    <div
                      v-for="(item, index) in mbInfoArr"
                      :key="index"
                      class="col-md-3 col-sm-12 col-xs-12"
                    >
                      <q-item
                        :style="`background-color: #3a9688`"
                        class="q-pa-none"
                      >
                        <q-item-section class="q-pa-md q-ml-none text-white">
                          <q-item-label>{{ index }}</q-item-label>
                          <q-item-label
                            class="text-white text-body1 text-weight-bolder"
                            >{{ item }}</q-item-label
                          >
                        </q-item-section>
                      </q-item>
                    </div>
                  </div>
                </q-card-section>
              </q-card>

              <br />
              

              <span
                v-if="jetsonInfoArr.length != 0"
                class="text-h6 q-mr-sm q-mb-sm q-pl-md"
                style="
                  width: auto;
                  border-left: 5px solid green;
                  background: rgba(220, 220, 220, 0.3);
                "
                >GPU Information</span
              >

              <q-card class="bg-transparent no-shadow no-border">
                <q-card-section class="q-pa-none">
                  <div class="row q-col-gutter-md">
                    <div
                      v-for="(item, index) in jetsonInfoArr"
                      :key="index"
                      class="col-md-3 col-sm-12 col-xs-12"
                    >
                      <q-item
                        :style="`background-color: #7cb342`"
                        class="q-pa-none"
                      >
                        <q-item-section class="q-pa-md q-ml-none text-white">
                          <q-item-label>{{ index }}</q-item-label>
                          <q-item-label
                            class="text-white text-body1 text-weight-bolder"
                            >{{ item }}</q-item-label
                          >
                        </q-item-section>
                      </q-item>
                    </div>
                  </div>
                </q-card-section>
              </q-card>

              <br />

              <!-- <span
                v-if="storageInfoArr.length != 0"
                class="text-h6 q-mr-sm q-mb-sm q-pl-md"
                style="
                  width: auto;
                  border-left: 5px solid green;
                  background: rgba(220, 220, 220, 0.3);
                "
                >Storage Infomation</span
              >

              <q-card class="bg-transparent no-shadow no-border">
                <q-card-section class="q-pa-none">
                  <div class="row q-col-gutter-md">
                    <div
                      v-for="(item, index) in storageInfoArr"
                      :key="index"
                      class="col-md-3 col-sm-12 col-xs-12"
                    >
                      <q-item
                        :style="`background-color: #7cb342`"
                        class="q-pa-none"
                      >
                        <q-item-section class="q-pa-md q-ml-none text-white">
                          <q-item-label>{{ index }}</q-item-label>
                          <q-item-label
                            class="text-white text-body1 text-weight-bolder"
                            >{{ item }}</q-item-label
                          >
                        </q-item-section>
                      </q-item>
                    </div>
                  </div>
                </q-card-section>
              </q-card> -->

              <br />
              <!-- <span
                v-if="mbInfoArr.length != 0"
                class="text-h6 q-mr-sm q-mb-sm q-pl-md"
                style="
                  width: auto;
                  border-left: 5px solid green;
                  background: rgba(220, 220, 220, 0.3);
                "
                >Motherboard Information</span
              >

              <q-card class="bg-transparent no-shadow no-border">
                <q-card-section class="q-pa-none">
                  <div class="row q-col-gutter-md">
                    <div
                      v-for="(item, index) in mbInfoArr"
                      :key="index"
                      class="col-md-3 col-sm-12 col-xs-12"
                    >
                      <q-item
                        :style="`background-color: #7cb342`"
                        class="q-pa-none"
                      >
                        <q-item-section
                          v-if="icon_position === 'right'"
                          side
                          :style="`background-color: #7cb342`"
                          class="q-pa-lg q-mr-none text-white"
                        >
                          <q-icon
                            :name="item.icon"
                            color="white"
                            size="24px"
                          ></q-icon>
                        </q-item-section>
                        <q-item-section class="q-pa-md q-ml-none text-white">
                          <q-item-label>{{ item.title }}</q-item-label>
                          <q-item-label
                            class="text-white text-body1 text-weight-bolder"
                            >{{ item.value }}</q-item-label
                          >
                        </q-item-section>
                      </q-item>
                    </div>
                  </div>
                </q-card-section>
              </q-card>
              <br /> -->
              <!-- <span
                v-if="cpuInfoArr.length != 0"
                class="text-h6 q-mr-sm q-mb-sm q-pl-md"
                style="
                  width: auto;
                  border-left: 5px solid green;
                  background: rgba(220, 220, 220, 0.3);
                "
                >CPU Information</span
              >
              <q-card class="bg-transparent no-shadow no-border">
                <q-card-section class="q-pa-none">
                  <div class="row q-col-gutter-md">
                    <div
                      v-for="(item, index) in cpuInfoArr"
                      :key="index"
                      class="col-md-3 col-sm-12 col-xs-12"
                    >
                      <q-item
                        :style="`background-color: ${item.color1}`"
                        class="q-pa-none"
                      >
                        <q-item-section
                          v-if="icon_position === 'right'"
                          side
                          :style="`background-color: ${item.color2}`"
                          class="q-pa-lg q-mr-none text-white"
                        >
                          <q-icon
                            :name="item.icon"
                            color="white"
                            size="24px"
                          ></q-icon>
                        </q-item-section>
                        <q-item-section class="q-pa-md q-ml-none text-white">
                          <q-item-label>{{ item.title }}</q-item-label>
                          <q-item-label
                            class="text-white text-body1 text-weight-bolder"
                            >{{ item.value }}</q-item-label
                          >
                        </q-item-section>
                      </q-item>
                    </div>
                  </div>
                  <br />
                </q-card-section>
              </q-card> -->
            </q-tab-panel>

            <q-tab-panel name="sensor">
              <q-card class="bg-transparent no-shadow no-border">
                <q-card-section class="q-pa-none">
                  <div class="row q-col-gutter-md">
                    I/O connected status / GPU / CPU / MEMORY
                    <div class="box" style="max-width: 360px">
                      <q-img src="../assets/dynamic_data.png"> </q-img>
                    </div>
                  </div>
                </q-card-section>
              </q-card>
            </q-tab-panel>

            <q-tab-panel name="movies">
              <div v-show="!isAIHide">Error</div>
              <div v-show="isAIHide">
                <q-table
                  :rows="rows"
                  :columns="columns"
                  row-key="name"
                  :key="tableKey"
                  :title="$t('ai_application')"
                  dark
                  color="amber"
                  separator="cell"
                >
                  <template v-slot:body-cell-name="props">
                    <q-td :props="props">
                      {{ props.value }}
                    </q-td>
                  </template>

                  <template v-slot:body-cell-carbs="props">
                    <q-td :props="props">
                      <div>
                        <q-badge
                          v-if="props.value == 'online'"
                          color="green"
                          :label="props.value"
                        />
                        <q-badge v-else color="blue" :label="props.value" />
                      </div>
                      <div class="my-table-details">
                        {{ props.row.details }}
                      </div>
                    </q-td>
                  </template>
                  <template v-slot:body-cell-protein="props">
                    <q-td :props="props">
                      <div>
                        <router-link to="/Pagination">
                          <q-btn
                            v-if="props.row.protein > props.row.carbs"
                            :size="xs"
                            no-caps
                            dense
                            color="green"
                            :label="'Upgrade to ' + props.value"
                          />
                        </router-link>
                        <q-badge
                          v-if="props.row.protein <= props.row.carbs"
                          :label="props.value"
                          color="grey"
                        />
                      </div>
                    </q-td>
                  </template>
                </q-table>
              </div>
            </q-tab-panel>
          </q-tab-panels>
        </q-card>

        <br />
      </div>
    </div>
  </q-page>
</template>

<script>
import { getFunc } from "src/boot/axios";
import { defineComponent, defineAsyncComponent } from "vue";
import { ref } from "vue";

const columns = [
  {
    name: "name",
    required: true,
    label: "Name",
    align: "left",
    field: (row) => row.name,
    format: (val) => `${val}`,
    sortable: true,
  },
  { name: "calories", label: "Type", field: "calories", sortable: true },
  { name: "fat", label: "Last updated Time", field: "fat", sortable: true },
  { name: "carbs", label: "Version", field: "carbs" },
  { name: "protein", label: "Lasted Version", field: "protein" },
  // { name: 'sodium', label: 'Sodium (mg)', field: 'sodium' },
  // { name: 'calcium', label: 'Calcium (%)', field: 'calcium', sortable: true, sort: (a, b) => parseInt(a, 10) - parseInt(b, 10) },
  // { name: 'iron', label: 'Iron (%)', field: 'iron', sortable: true, sort: (a, b) => parseInt(a, 10) - parseInt(b, 10) }
];
const rows_b = [
  {
    name: "TAO",
    calories: "XXX",
    fat: "02/25/2022 @ 7:36am",
    carbs: "v1.0.2",
    protein: "v1.0.2",
    sodium: 87,
    calcium: "14%",
    iron: "1%",
  },
  {
    name: "TRITON",
    calories: "YYY",
    fat: "01/25/2021 @ 3:12pm",
    carbs: "v1.3.2",
    protein: "v1.3.4",
    sodium: 129,
    calcium: "8%",
    iron: "1%",
  },
  {
    name: "DEEPSTREAM",
    calories: "ZZZ",
    fat: "01/09/2020 @ 5:35pm",
    carbs: "v1.2.0",
    protein: "v1.2.0",
    sodium: 337,
    calcium: "6%",
    iron: "7%",
  },
];

export default defineComponent({
  name: "UserProfile",
  setup() {
    return {
      model: ref("Frequency"),
      options: ["Frequency", "Temperature", "Usage", "Voltage"],
      tmpDs: [],
      user_details: {},
      password_dict: {},
      tab: ref("mails"),
      isAIHide: ref(false),
      columns,
      rows: ref(rows_b),
      overview: ref({}),
      overviewArr: ref([]),
      libArr: ref([]),
      osArr: ref([]),
      mbInfoArr: ref([]),
      cpuInfoArr: ref([]),
      netInfoArr: ref([]),
      jetsonInfoArr: ref([]),
      storageInfoArr: ref([]),
      newIdxChartDataArr: ref([]),
      newChartDataArr: ref([]),
      cpuDynamic: ref({}),
      devStatus: ref(false),
      //devIp: ref(""),
    };
  },
  watch: {
    model: function () {
      console.log(this.model);
      //console.log(this.tmpDs.length);
      console.log(this.tmpDs[this.model]);
      console.log(this.tmpDs[this.model].label);

      // if(!this.tmpDs.length)
      // {
      //   return;
      // }

      this.newChartDataArr.length = 0;
      this.newChartDataArr.push(this.tmpDs[this.model]);
    },
  },
  mounted() {
    // this.tmpDs["Frequency"].length = 0
    // this.tmpDs["Temperature"].length = 0
    // this.tmpDs["Usage"].length = 0
    // this.tmpDs["Voltage"].length = 0
    this.tmpDs.length = 0;
    this.newChartDataArr.length = 0;
    this.newIdxChartDataArr["cpu"] = {
      labels: [
        "1 January",
        "2 February",
        "3 March",
        "4 April",
        "5 May",
        "6 June",
        "7 July",
      ],
      datasets: [
        {
          label: "Data 1",
          backgroundColor: "#f87979",
          data: [40, 39, 10, 40, 39, 80, 40],
        },
        {
          label: "Data 2",
          backgroundColor: "#f87900",
          data: [145, 144, 113, 149, 140, 182, 147],
        },
      ],
    };
  },
  created() {
    console.log("PROFIEL created()");
    console.log(this.$route.params);
    this.devId = this.$route.params.devId;
    this.devName = this.$route.params.devName;
    this.devIp = "";
    this.getDevStatus();
    this.getDevInfo();
    this.getDevDynamicData();
  },

  data() {
    return {
      newIdxChartDataArr: [],
      newChartDataArr: [
        {
          label: "CPU Temp",
          labels: [
            "January",
            "February",
            "March",
            "April",
            "May",
            "June",
            "July",
          ],
          datasets: [
            {
              label: "Data One",
              backgroundColor: "#f87979",
              data: [40, 39, 10, 40, 39, 80, 40],
            },
            //           {
            //   label: 'Data TWO',
            //   backgroundColor: '#f87900',
            //   data: [145, 144, 113, 149, 140, 182, 147]
            // }
          ],
        },
        {
          label: "GPU Temp",
          labels: ["1", "2", "3", "4", "5", "6", "7"],
          datasets: [
            {
              label: "Data One",
              backgroundColor: "#f87979",
              data: [40, 30, 21, 40, 31, 18, 14],
            },
            //           {
            //   label: 'Data TWO',
            //   backgroundColor: '#f87900',
            //   data: [145, 144, 113, 149, 140, 182, 347]
            // }
          ],
        },
      ],
      chartData: {
        labels: [
          "January",
          "February",
          "March",
          "April",
          "May",
          "June",
          "July",
        ],
        datasets: [
          {
            label: "Data One",
            backgroundColor: "#f87979",
            data: [40, 39, 10, 40, 39, 80, 40],
          },
          {
            label: "Data TWO",
            backgroundColor: "#f87900",
            data: [145, 144, 113, 149, 140, 182, 147],
          },
        ],
      },
      newChartOptionsArr: [
        {
          responsive: true,
          maintainAspectRatio: false,
        },
      ],
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
      },
      chartdata: {
        labels: [],
        datasets: [
          {
            label: "0050",
            fill: false,
            backgroundColor: "rgba(188,96,147,1)",
            borderColor: "rgba(212,85,49,1)",
            pointBackgroundColor: "rgba(41,56,69,1)",
            data: [],
          },
        ],
      },
      options: {
        title: {
          display: true,
          fontSize: 24,
          text: "2020/11 0050 價格走勢",
        },
      },
      devAlias: "",
      customChartData: {
        labels: [
          "January",
          "February",
          "March",
          "April",
          "May",
          "June",
          "July",
        ],
        datasets: [
          {
            label: "Data 1",
            data: [2, 10, 5, 9, 0, 6, 20],
            backgroundColor: this.gradient,
            borderColor: "#FC2525",
            pointBackgroundColor: "rgba(171, 71, 188, 1)",
          },
        ],
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
        title: {
          display: true,
          text: "My Data",
        },
      },
    };
  },
  methods: {
    update() {
      setTimeout(() => {
        console.log("update");

        this.customChartData.datasets[0] = {
          label: "Data 1",
          data: [12, 10, 15, 19, 10, 16, 20],
          backgroundColor: "transparent",
          borderColor: "#FC2525",
          pointBackgroundColor: "rgba(171, 71, 188, 1)",
        };

        this.customChartData = {
          ...this.customChartData,
        };
      }, 2000);
    },
    toggleAI() {
      this.isAIHide = !this.isAIHide;
    },
    clickUpgradeAI(val) {
      console.log("clickUpgradeAI" + val);
    },
    async getDevStatus() {
      const devStatusUrl = "/devices/" + this.devId + "/status";
      try {
        this.devStatus = "offline";
        const response = await getFunc(devStatusUrl);
        // console.log("getDevStatusgetDevStatus");
        // console.log(response.data.data.status);
        this.devStatus = response.data.data.status;
      } catch (error) {
        throw new Error(error);
      }
    },
    async getDevDynamicData() {
      const dateTime = Date.now();
      //const timestamp = Math.floor(dateTime / 1000);
      console.log("=getDevDynamicDatagetDevDynamicDatagetDevDynamicData=");
      console.log(dateTime);
      console.log(dateTime - 48 * 60 * 1000);

      const devDynamicDataUrl =
        "/devices/" +
        this.devId +
        "/data/dynamic?limit=10&useStrictDataTypes=false&keys=CPU&startTs=" +
        "1652237850339" +
        "&endTs=" +
        dateTime;
      const response = await getFunc(devDynamicDataUrl);
      //console.log("CPU.length:");
      //console.log(response.data.data.CPU.length);
      //console.log(response.data.data.CPU);
      console.log("ddddddd");
      console.log(this.cpuDynamic.CPU);
      console.log(response.data.data); // TINA
      console.log("lenght : " + response.data.data.CPU.length);
      for( var k = 0 ; k < response.data.data.CPU.length; k++) {
      console.log(response.data.data.CPU[k].value);
      }
      //this.cpuDynamic.CPU = response.data.data.CPU[0].value
      var obj = JSON.parse(response.data.data.CPU[0].value);
      console.log(obj);
      this.cpuDynamic.CPU = obj;

      if (!response.data.data.CPU) {
        return;
      }
      //this.cpuDynamic = response.data.data
      var label = "CPU Frequency";
      var labels = [];
      var data = [];
      var tempData = [];
      var usageData = [];
      var vData = [];

      //label = "CPU Frequency"
      var datasets = [];
      var tempDatasets = [];
      var usageDatasets = [];
      var vDatasets = [];
      var j = 6;
      console.log(this.coreNum);
      for (var j = 0; j < 6; j++) {
        data = [];
        data.length = 0;
        tempData = [];
        tempData.length = 0;
        usageData = [];
        usageData.length = 0;
        vData = [];
        vData.length = 0;
        labels = [];
        labels.length = 0;

        response.data.data.CPU.forEach(function (item, i) {
          //console.log(i, item)
          //console.log(item.value)
          var obj = JSON.parse(item.value);
          console.log("++++");
          console.log(obj);
        });

// for (var i = response.data.data.CPU.length; i > 0; i--) {
//           console.log(response.data.data.CPU[i])
// }
        //for (var i = 0; i < response.data.data.CPU.length; i++) {
        for (var i = (response.data.data.CPU.length-1); i >= 0; i--) {
          //console.log(response.data.data.CPU[i])
          // if (!response.data.data.CPU[i].hasOwnProperty('ts')) {
          //   continue;
          // }
          var spe_d4 = new Date(response.data.data.CPU[i].ts); // timestamp millisecond

          // console.log(
          //   spe_d4.getHours(),
          //   spe_d4.getMinutes(),
          //   spe_d4.getSeconds()
          // );
          var dateStr =
            spe_d4.getHours() +
            ":" +
            spe_d4.getMinutes() +
            ":" +
            spe_d4.getSeconds();

          labels.push(dateStr);

          var obj = JSON.parse(response.data.data.CPU[i].value);

          console.log(obj[j].Freq);
          data.push(obj[j].Freq);

          tempData.push(obj[j].Temp);
          usageData.push(obj[j].Usage);
          console.log("core " + j + "  : " + obj[j].Usage);

          vData.push(obj[j].V);

          var datasetSub = {
            label: "Core " + j,
            backgroundColor: "#00396b", //"#f8" + j + "979", //"#f87979",
            data: data, //[40, 39, 10, 40, 39, 80, 40],
          };

          var tempDatasetSub = {
            label: "Core " + j,
            backgroundColor: "#f87979", //"#f8" + j + "979", //"#f87979",
            data: tempData, //[40, 39, 10, 40, 39, 80, 40],
          };

          var usageDatasetSub = {
            label: "Core " + j,
            backgroundColor: "#00596b", //"#f87979",
            data: usageData, //[40, 39, 10, 40, 39, 80, 40],
          };

          var vDatasetSub = {
            label: "Core " + j,
            backgroundColor: "#f80079", //"#f87979",
            data: vData, //[40, 39, 10, 40, 39, 80, 40],
          };
        }
        datasets.push(datasetSub);
        tempDatasets.push(tempDatasetSub);
        usageDatasets.push(usageDatasetSub);
        vDatasets.push(vDatasetSub);
        console.log(data);
      }
      this.ds = {
        label: "CPU Frequency",
        labels: labels,
        datasets: datasets,
      };

      this.tempDs = {
        label: "CPU Temperature",
        labels: labels,
        datasets: tempDatasets,
      };

      this.usageDs = {
        label: "CPU Usage",
        labels: labels,
        datasets: usageDatasets,
      };

      this.vDs = {
        label: "CPU Voltage",
        labels: labels,
        datasets: vDatasets,
      };
      console.log("===ds===");
      console.log(this.ds);
      console.log("===newChartDataArr===");
      console.log(this.newChartDataArr);
      this.newChartDataArr.length = 0;
      this.newChartDataArr.push(this.ds);

      //this.newChartDataArr["Frequency"] = this.ds;
      //this.tmpDs = []
      this.tmpDs.length = 0;
      //       this.tmpDs["Frequency"] = {}
      // this.tmpDs["Temperature"]= {}
      // this.tmpDs["Usage"]= {}
      // this.tmpDs["Voltage"]= {}

      this.tmpDs["Frequency"] = this.ds;
      this.tmpDs["Temperature"] = this.tempDs;
      this.tmpDs["Usage"] = this.usageDs;
      this.tmpDs["Voltage"] = this.vDs;

      //console.log(data)
      console.log("UUUUUUUsage");
      console.log(this.tmpDs["Usage"]);
      //this.tmpDs["Usage"] = {}
      console.log(this.tmpDs["Usage"]);
      //console.log(this.tmpDs["Voltage"])
      // tina
      //this.newChartDataArr.length = 0;
      //this.newChartDataArr.push(this.tmpDs["Voltage"]);

      console.log("====newChartDataArr====");
      console.log(this.newChartDataArr);

      ///devices/8001d790-ae6f-11ec-9628-91d5e16b33a3/data/dynamic?limit=100&useStrictDataTypes=false&keys=CPU&startTs=1652237850339&endTs=1652767861288
    },
    async getDevInfo() {
      console.log("getDevInfo()");
      try {
        const devStaticDataUrl = "/devices/" + this.devId + "/data/static";
        const response = await getFunc(devStaticDataUrl);
        //console.log(response.data.data.list);
        var i = 0;
        var ipAddr = "";
        this.libArr.length = 0;
        this.osArr.length = 0;
        this.mbInfoArr.length = 0;
        this.cpuInfoArr.length = 0;
        this.netInfoArr.length = 0;
        this.jetsonInfoArr.length = 0;
        this.storageInfoArr.length = 0;

        //this.cpuDynamic.CPU.length = 0;
        for (i = 0; i < response.data.data.list.length; i++) {
          if ("overview" == response.data.data.list[i].key) {
            console.log(response.data.data.list[i].value);
            this.overviewArr = response.data.data.list[i].value;
          } else if ("alias" == response.data.data.list[i].key) {
            this.devAlias = response.data.data.list[i].value;
          } else if ("library_version" == response.data.data.list[i].key) {
            this.libArr = response.data.data.list[i].value;
          } else if ("Sys" == response.data.data.list[i].key || "os_version" == response.data.data.list[i].key) {
            this.osArr = response.data.data.list[i].value;
          } else if ("MB" == response.data.data.list[i].key || "mb_info" == response.data.data.list[i].key) {
            this.mbInfoArr = response.data.data.list[i].value;
          } else if ("CPU" == response.data.data.list[i].key || "cpu_info" == response.data.data.list[i].key) {
            this.cpuInfoArr = response.data.data.list[i].value;
          } else if ("GPU" == response.data.data.list[i].key || "jetson_info" == response.data.data.list[i].key) {
            this.jetsonInfoArr = response.data.data.list[i].value;
          } else if ("Net" == response.data.data.list[i].key) {
            this.netInfoArr = response.data.data.list[i].value;
          } else if ("Storage" == response.data.data.list[i].key) {
            this.storageInfoArr = response.data.data.list[i].value;
          }
        }
        console.log("+++++++++CPU");
        console.log(this.cpuInfoArr.Numofcore);
        this.coreNum = this.cpuInfoArr.Numofcore;
        //console.log(this.netInfoArr.length)

        for (var i = 0; i < this.netInfoArr.length; i++) {
          console.log(this.netInfoArr[i].IPaddr);
          if (this.netInfoArr[i].IPaddr) {
            this.devIp = this.netInfoArr[i].IPaddr;
          }
        }

        //         this.netInfoArr.forEach(function(item, i) {

        //   console.log(item.IPaddr)
        //   if(item.IPaddr != '') {
        //     ipAddr = item.IPaddr
        //   }
        //   console.log(this.devIp)
        // });

        // this.cpuDynamic = {
        //   CPU: {
        //     0: {
        //       Freq: 1381,
        //       Temp: 61,
        //       Usage: 53.9453353881836,
        //       V: 4.99200010299683,
        //     },
        //     1: {
        //       Freq: 1384,
        //       Temp: 61,
        //       Usage: 54.2137832641602,
        //       V: 4.99200010299683,
        //     },
        //     2: {
        //       Freq: 1381,
        //       Temp: 61,
        //       Usage: 53.9179611206055,
        //       V: 4.99200010299683,
        //     },
        //     3: {
        //       Freq: 1381,
        //       Temp: 61,
        //       Usage: 53.9123992919922,
        //       V: 4.99200010299683,
        //     },
        //     4: {
        //       Freq: 1381,
        //       Temp: 61,
        //       Usage: 54.7151298522949,
        //       V: 4.99200010299683,
        //     },
        //     5: {
        //       Freq: 1382,
        //       Temp: 61,
        //       Usage: 54.7243881225586,
        //       V: 4.99200010299683,
        //     },
        //     FanRPM: 160,
        //     Freq: 1907,
        //     Usage: 39.4000015258789,
        //   },
        // };

        // console.log(this.cpuDynamic["CPU"].length);
        // console.log(this.cpuDynamic.CPU[0]);
        // console.log(this.cpuDynamic.CPU[3]);
        //console.log(this.newChartDataArr[0]);
      } catch (error) {
        throw new Error(error);
      }
      //await getFunc()
    },
  },
  components: {
    PieChart: defineAsyncComponent(() => import("components/charts/PieChart")),
    AreaChart: defineAsyncComponent(() =>
      import("components/charts/AreaChart")
    ),
    BarChartV: defineAsyncComponent(() =>
      import("components/charts/BarChartV")
    ),
    LineChartV: defineAsyncComponent(() =>
      import("components/charts/LineChartV")
    ),
    MapMarker: defineAsyncComponent(() => import("pages/MapMarker")),
  },
  props: {
    icon_position: {
      required: false,
      default: "left",
    },
  },
  computed: {
    mbInfo: function () {
      return [
        {
          title: "Manufacturer",
          icon: "fas fa-dollar-sign",
          value: "Aetina Corporation.",
          color1: "#7cb342",
          color2: "#3e51b5",
        },
        {
          title: "Product name",
          icon: "fas fa-chart-bar",
          value: "AIE-CN11",
          color1: "#3a9688",
          color2: "#3e51b5",
        },
        {
          title: "Serial Number",
          icon: "fas fa-chart-line",
          value: "1422221004911",
          color1: "#7cb342",
          color2: "#3e51b5",
        },
        {
          title: "BIOS Manufacturer",
          icon: "person",
          value: "Aetina Corporation.",
          color1: "#3a9688",
          color2: "#3e51b5",
        },
      ];
    },
    bsp_ver: function () {
      return [
        {
          title: "Aetina version",
          icon: "fas fa-dollar-sign",
          value: "R32_4_4_Xavier",
          color1: "#7cb342",
          color2: "#3e51b5",
        },
        {
          title: "Manufacturer version",
          icon: "fas fa-chart-bar",
          value: "R32.4.4",
          color1: "#3a9688",
          color2: "#3e51b5",
        },
        {
          title: "TensorRT",
          icon: "fas fa-chart-line",
          value: "NOT_INSTALLED",
          color1: "#7cb342",
          color2: "#3e51b5",
        },
        {
          title: "Visionwork",
          icon: "person",
          value: "NOT_INSTALLED",
          color1: "#3a9688",
          color2: "#3e51b5",
        },
      ];
    },
    jetson_stats: function () {
      return [
        {
          title: "Power Mode",
          icon: "fas fa-chart-bar",
          value: "MODE_10W_DESKTOP (Type:5)",
          color1: "#3a9688",
          color2: "#3e51b5",
        },
      ];
    },
    pack_ver: function () {
      return [
        {
          title: "CUDA",
          icon: "fas fa-dollar-sign",
          value: "NOT_INSTALLED",
          color1: "#7cb342",
          color2: "#3e51b5",
        },
        {
          title: "cuDNN",
          icon: "fas fa-chart-bar",
          value: "NOT_INSTALLED",
          color1: "#7cb342",
          color2: "#3e51b5",
        },
        {
          title: "TensorRT",
          icon: "fas fa-chart-line",
          value: "NOT_INSTALLED",
          color1: "#7cb342",
          color2: "#3e51b5",
        },
        {
          title: "OpenCV",
          icon: "fas fa-chart-line",
          value: "NOT_INSTALLED",
          color1: "#7cb342",
          color2: "#3e51b5",
        },
      ];
    },
    cpuInfo: function () {
      return [
        {
          title: "Manufacturer",
          icon: "fas fa-dollar-sign",
          value: "Nvidia",
          color1: "#7cb342",
          color2: "#3e51b5",
        },
        {
          title: "Name",
          icon: "fas fa-chart-bar",
          value: "ARMv8 Processor rev 0 (v8l)",
          color1: "#3a9688",
          color2: "#3e51b5",
        },
        {
          title: "Number of cores",
          icon: "fas fa-chart-line",
          value: "6",
          color1: "#7cb342",
          color2: "#3e51b5",
        },
        {
          title: "L2 Cache size",
          icon: "person",
          value: "2048 KB",
          color1: "#3a9688",
          color2: "#3e51b5",
        },
      ];
    },
    items: function () {
      return this.icon_position === "left"
        ? [
            {
              title: "Operating System",
              icon: "fab fa-ubuntu",
              value: "Ubuntu 18.04.6 LTS",
              // color1: "#5064b5",
              // color2: "#3e51b5",
              color1: "#3a9688",
              color2: "#3a9688",
            },
            {
              title: "Jetpack",
              icon: "fab fa-ubuntu",
              value: "4.6.1 (L4T 32.7.1)",
              // color1: "#5064b5",
              //     color2: "#3e51b5",
              color1: "#3a9688",
              color2: "#3a9688",
            },
            {
              title: "kernel",
              icon: "fab fa-ubuntu",
              value: "Linux 4.9.201-tegra aarch64",
              color1: "#3a9688",
              color2: "#3a9688",
              // color1: "#ea6a7f",
              // color2: "#ea4b64",
            },
          ]
        : [
            {
              title: "Monthly Income",
              icon: "fas fa-dollar-sign",
              value: "$ 20k",
              color1: "#546bfa",
              color2: "#3e51b5",
            },
            {
              title: "Weekly Sales",
              icon: "fas fa-chart-bar",
              value: "20",
              color1: "#3a9688",
              color2: "#3e51b5",
            },
            {
              title: "New Customers",
              icon: "fas fa-chart-line",
              value: "321",
              color1: "#7cb342",
              color2: "#3e51b5",
            },
            {
              title: "Active Users",
              icon: "person",
              value: "82",
              color1: "#f88c2b",
              color2: "#3e51b5",
            },
          ];
    },
  },
});

Date.prototype.format = function (fmt) {
  var o = {
    "M+": this.getMonth() + 1, //月份
    "d+": this.getDate(), //日
    "h+": this.getHours(), //小時
    "m+": this.getMinutes(), //分
    "s+": this.getSeconds(), //秒
    "q+": Math.floor((this.getMonth() + 3) / 3), //季度
    S: this.getMilliseconds(), //毫秒
  };
  if (/(y+)/.test(fmt))
    fmt = fmt.replace(
      RegExp.$1,
      (this.getFullYear() + "").substr(4 - RegExp.$1.length)
    );
  for (var k in o)
    if (new RegExp("(" + k + ")").test(fmt))
      fmt = fmt.replace(
        RegExp.$1,
        RegExp.$1.length == 1 ? o[k] : ("00" + o[k]).substr(("" + o[k]).length)
      );
  return fmt;
};
</script>

<style scoped>
.card-bg {
  background-color: #162b4d;
}
</style>

