<template>
  <q-page class="q-pa-sm bg-white">
    <q-resize-observer @resize="onResize" />

    <div class="row" v-if="!$q.screen.lt.sm">
      <div class="col-lg-4 col-md-4 col-sm-12 col-xs-12">
        <q-card class="no-border no-border">
          <q-tab-panels v-model="tab" animated class="bg-white text-black">
            <q-tab-panel
              name="all"
              class="q-pa-none full-height"
              :style="{ height: size['height'] - 80 + 'px !important' }"
            >
              <q-list class="">
                <q-toolbar class="bg-teal-10 text-white">
                  <q-toolbar-title>
                    {{ contacts_list.length }} GROUP(S)
                  </q-toolbar-title>

                  <q-input
                    dense
                    rounded
                    outlined
                    bg-color="white"
                    v-model="search"
                  >
                    <template v-slot:append>
                      <q-icon name="search" />
                    </template>
                  </q-input>
                  <q-btn
                    class="text-black q-ml-sm"
                    icon="add"
                    color="white"
                    @click="prompt = true"
                    padding="sm"
                    dense
                    size="xs"
                  />
                </q-toolbar>
                <span
                  v-for="(contact, index) in contacts_list"
                  :key="index"
                  @click="
                    selected_contact = contact;
                    clickGroup(contact.id);
                  "
                >
                  <contact-item
                    :avatar="contact.avatar"
                    :name="contact.name"
                    :position="contact.position"
                  ></contact-item>
                </span>
              </q-list>
            </q-tab-panel>

            <q-tab-panel
              name="favorites"
              class="q-pa-none"
              :style="{ height: size['height'] - 80 + 'px !important' }"
            >
              <q-list class="">
                <q-item-label class="text-center q-pa-sm">
                  <q-input dense rounded outlined v-model="search">
                    <template v-slot:append>
                      <q-icon name="search" />
                    </template>
                  </q-input>
                </q-item-label>
                <q-item-label header class="text-center"
                  >{{ favorites_list.length }} Favorites</q-item-label
                >

                <span
                  v-for="(favorite, index) in favorites_list"
                  :key="index"
                  @click="selected_contact = favorite"
                >
                  <contact-item
                    :avatar="favorite.avatar"
                    :name="favorite.name"
                    :position="favorite.position"
                  ></contact-item>
                </span>
              </q-list>
            </q-tab-panel>
          </q-tab-panels>

          <q-tabs v-model="tab" dense class="bg-grey-3" align="justify">
            <q-tab
              name="all"
              icon="group"
              class="text-capitalize"
              label="All"
            ></q-tab>
            <q-tab
              name="favorites"
              icon="star"
              class="text-capitalize"
              label="Favorites"
            ></q-tab>
          </q-tabs>
        </q-card>
      </div>
      <div class="col-lg-8 q-pl-xs col-md-8 col-sm-12 col-xs-12">
        <q-card
          class="no-border no-border"
          bg-color="black"
          :style="{ height: size['height'] - 24 + 'px !important' }"
        >
          <q-toolbar class="bg-teal-10 text-white">
            <q-btn round flat class="q-pa-sm">
              <q-avatar size="60px" color="white">
                <img :src="selected_contact.avatar" />
              </q-avatar>
            </q-btn>

            <q-item class="q-subtitle-1 q-pl-md">
              <q-item-section>
                <q-item-label lines="1" style="font-size: 20px">{{
                  selected_contact.name
                }}</q-item-label>
                <q-item-label caption lines="2">
                  <span class="text-weight-bold text-white">{{
                    selected_contact.position
                  }}</span>
                </q-item-label>
              </q-item-section>
            </q-item>

            <q-space />
            <q-btn
              round
              flat
              icon="delete"
              color="white"
              @click="deleteGroup(selected_contact.id)"
              size="md"
            />
          </q-toolbar>
          <q-separator></q-separator>

          <div>
            <q-table
              :title="$t('devices')"
              :rows="rows"
              :columns="columns"
              row-key="name"
              :key="tableKey"
              :selected-rows-label="getSelectedString"
              selection="multiple"
              v-model:selected="selected"
            >
              <!-- dark class="bg-indigo-8" -->

              <template v-slot:top-right class="q-pa-sm">
                <q-btn
                  v-if="selected.length != 0"
                  color="primary"
                  label="Delete Device"
                  no-caps
                  @click="promptDelDeviceConfirm = true"
                />
                <q-btn
                  color="primary"
                  label="Add Device"
                  no-caps
                  @click="promptDevice = true"
                  style="margin: 5px"
                />
              </template>
              <template v-slot:append class="q-pa-sm">
                <q-btn
                  v-if="selected.length != 0"
                  color="primary"
                  label="Delete Device"
                  no-caps
                  @click="promptDelDeviceConfirm = true"
                />
                <q-btn
                  color="primary"
                  label="Add Device"
                  no-caps
                  @click="promptDevice = true"
                />
              </template>

              <template v-slot:body-cell-name="props">
                <q-td :props="props">
                  <router-link
                    :to="{
                      name: 'PRO',
                      params: {
                        devId: props.row.calories,
                        devName: props.value,
                      },
                    }"
                  >
                    {{ props.value }}
                  </router-link>
                  <!-- <router-link to="/Profile">{{ props.value }}</router-link> -->
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
                    <q-badge v-else color="red" :label="props.value" />
                  </div>
                  <div class="my-table-details">
                    {{ props.row.details }}
                  </div>
                </q-td>
              </template>
            </q-table>
          </div>
        </q-card>
      </div>
    </div>

    <div v-else>
      <div v-if="Object.keys(selected_contact).length == 0">
        <q-tab-panels v-model="tab" animated class="bg-white">
          <q-tab-panel
            name="all"
            class="q-pa-none full-height"
            :style="{ height: size['height'] - 100 + 'px !important' }"
          >
            <q-list class="">
              <q-item-label class="text-center q-pa-sm">
                <q-input dense rounded outlined v-model="search">
                  <template v-slot:append>
                    <q-icon name="search" />
                  </template>
                </q-input>
              </q-item-label>
              <q-item-label header class="text-center"
                >{{ contacts_list.length }} CONTACTS</q-item-label
              >

              <span
                v-for="(contact, index) in contacts_list"
                :key="index"
                @click="selected_contact = contact"
              >
                <contact-item
                  :avatar="contact.avatar"
                  :name="contact.name"
                  :position="contact.position"
                ></contact-item>
              </span>
            </q-list>
          </q-tab-panel>

          <q-tab-panel
            name="favorites"
            class="q-pa-none"
            :style="{ height: size['height'] - 80 + 'px !important' }"
          >
            <q-list class="">
              <q-item-label class="text-center q-pa-sm">
                <q-input dense rounded outlined v-model="search">
                  <template v-slot:append>
                    <q-icon name="search" />
                  </template>
                </q-input>
              </q-item-label>
              <q-item-label header class="text-center"
                >{{ favorites_list.length }} Favorites</q-item-label
              >

              <span
                v-for="(favorite, index) in favorites_list"
                :key="index"
                @click="selected_contact = favorite"
              >
                <contact-item
                  :avatar="favorite.avatar"
                  :name="favorite.name"
                  :position="favorite.position"
                ></contact-item>
              </span>
            </q-list>
          </q-tab-panel>
        </q-tab-panels>
        <q-tabs v-model="tab" dense class="bg-grey-3" align="justify">
          <q-tab
            name="all"
            icon="person"
            class="text-capitalize"
            label="All"
          ></q-tab>
          <q-tab
            name="favorites"
            icon="star"
            class="text-capitalize"
            label="Favorites"
          ></q-tab>
        </q-tabs>
      </div>
      <transition v-else appear enter-active-class="animated bounceInRight">
        <q-card
          class="no-border no-border"
          :style="{ height: size['height'] - 100 + 'px !important' }"
        >
          <q-toolbar class="text-black">
            <q-item class="q-subtitle-1 q-pl-md">
              <q-item-section>
                <q-item-label lines="1">{{
                  selected_contact.name
                }}</q-item-label>
                <q-item-label caption lines="2">
                  <span class="text-weight-bold">{{
                    selected_contact.position
                  }}</span>
                </q-item-label>
              </q-item-section>
            </q-item>

            <q-space />

            <q-btn round flat icon="star_outline" color="yellow"> </q-btn>
            <q-btn
              round
              flat
              icon="keyboard_backspace"
              @click="selected_contact = {}"
            />
          </q-toolbar>
          <q-separator></q-separator>
        </q-card>
      </transition>
    </div>
  </q-page>
  <q-dialog v-model="prompt" persistent>
    <q-card style="min-width: 350px">
      <q-card-section>
        <div class="text-h6">Group name</div>
      </q-card-section>

      <q-card-section class="q-pt-none">
        <q-input
          dense
          v-model="group_name"
          autofocus
          @keyup.enter="prompt = false"
        />
      </q-card-section>

      <q-card-actions align="right" class="text-primary">
        <q-btn flat label="Cancel" v-close-popup />
        <q-btn flat label="Add" v-close-popup @click="addGroup" />
      </q-card-actions>
    </q-card>
  </q-dialog>

  <q-dialog v-model="promptDelDeviceConfirm" persistent>
    <q-card style="min-width: 350px">
      <q-card-section>
        <div class="text-h6">Would you delete these devices?</div>
      </q-card-section>

      <q-card-section>
        <div v-for="(favorite, index) in selected" :key="index">
          {{ favorite.name }}
        </div>
      </q-card-section>

      <q-card-actions align="right" class="text-primary">
        <q-btn flat label="Cancel" v-close-popup />
        <q-btn flat label="Delete" v-close-popup @click="delDevice" />
      </q-card-actions>
    </q-card>
  </q-dialog>

  <q-dialog v-model="promptDevice" persistent>
    <q-card style="min-width: 350px">
      <q-card-section>
        <div class="text-h6">Add Device</div>
      </q-card-section>

      <q-card-section class="q-pt-none">
        <div class="q-pa-md">
          <div class="q-gutter-md" style="max-width: 300px">
            <q-input
              dense
              v-model="device_name"
              autofocus
              :label="$t('device_name')"
              filled
              color="teal-7"
              label-color="teal-7"
            />
            <q-input
              dense
              v-model="device_type"
              :label="$t('device_type')"
              filled
              color="teal-7"
              label-color="teal-7"
            />
            <q-input
              dense
              v-model="device_alias"
              :label="$t('device_alias')"
              filled
              color="teal-7"
              label-color="teal-7"
            />
          </div>
        </div>
      </q-card-section>

      <q-card-actions align="right" class="text-primary">
        <q-btn flat label="Cancel" v-close-popup />
        <q-btn flat label="Add" v-close-popup @click="addDevice" />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script>
import { defineComponent, defineAsyncComponent } from "vue";
import { useQuasar } from "quasar";
import { ref } from "vue";
import axios from "axios";
import { getFunc, postFunc } from "../boot/axios.js";

const favorites_list = [
  {
    name: "Jetson series",
    position: "02/25/2022 @ 2:47am",
    avatar:
      "https://www.aetina.com/wp-content/uploads/solution/object_detection/feature_flexible_deployment.svg",
    email: "pratikpatelpp802@gmail.com",
    company_email: "pratikpatelpp802@gmail.com",
    website: "www.test.com",
    phone: "+9910101010",
    secondary_phone: "+9910101010",
    address: "BB 101 Om Sai Residency Palsana",
  },
  {
    name: "Others",
    position: "02/25/2022 @ 2:47am",
    avatar:
      "https://www.aetina.com/wp-content/uploads/solution/object_detection/feature_integration.svg",
    email: "mailto:razvan@quasar.dev",
    company_email: "mailto:razvan@quasar.dev",
    website: "https://github.com/rstoenescu",
    phone: "+1-004-658-0042",
    secondary_phone: "(331) 009-4655 x3147",
    address: "92290 Lisa Cove",
  },
  {
    name: "Aetina-Yilan",
    position: "Location",
    avatar:
      "https://www.aetina.com/wp-content/uploads/solution/object_detection/feature_integration.svg",
    email: "mailto:jeff@quasar.dev",
    company_email: "mailto:jeff@quasar.dev",
    website: "http://jeffgalbraith.dev/",
    phone: "175.718.4633 x878",
    secondary_phone: "175.718.4633 x878",
    address: "Calgary, Canada",
  },
];

const rows_b = [];
var rows = [];

export default defineComponent({
  name: "Contact",
  components: {
    ContactDetailItem: defineAsyncComponent(() =>
      import("components/ContactDetailItem")
    ),
    ContactItem: defineAsyncComponent(() => import("components/ContactItem")),
  },
  setup() {
    const $q = useQuasar();
    const size = ref({ width: "200px", height: "200px" });
    const clonedData = {};
    const selected = ref([]);

    return {
      tab: ref("all"),
      search: ref(""),
      size,
      contacts_list: ref({}),
      favorites_list,
      selected_contact: ref({}),
      group_name: ref(""),
      device_name: ref(""),
      device_type: ref(""),
      device_alias: ref(""),
      selected,

      getSelectedString() {
        return selected.value.length === 0
          ? ""
          : `${selected.value.length} record${
              selected.value.length > 1 ? "s" : ""
            } selected of ${rows.length}`;
      },
      onResize(size_dynamic) {
        size.value = size_dynamic;
      },
      columns: ref([
        {
          name: "name",
          required: true,
          label: "Name", //getWord("name"),// => this.$t("name"),//"Name",  () => this.$t("label")
          align: "left",
          field: (row) => row.name,
          format: (val) => `${val}`,
          sortable: true,
        },

        { name: "calories", label: "ID", field: "calories", sortable: true },
        { name: "fat", label: "Created Time", field: "fat", sortable: true },
        { name: "carbs", label: "Status", field: "carbs", sortable: true },
        // { name: "protein", label: "Alias", field: "protein", sortable: true },
      ]),
      rows: ref(rows_b),

      tableKey: 0,
      currentGroup: 0,
      time: null,
      prompt: ref(false),
      promptDevice: ref(false),
      promptDelDeviceConfirm: ref(false),
    };
  },
  watch: {
    search: function () {
      console.log(this.search);
      const tmpSearch = this.search;
      var tmpRows,
        oriRows = new Object();

      tmpRows = this.contacts_list;
      oriRows = this.contacts_list;
      if (this.search.length == 0) {
        this.getGroup();
      } else {
        this.contacts_list = this.clonedData.filter(function (el) {
          console.log(tmpSearch);
          return el.name.includes(tmpSearch);
        });
      }
    },
  },

  methods: {
    async getGroupByFileter() {
      this.getGroup();
      this.contacts_list = await tmpRows.filter(function (el) {
        console.log(tmpSearch);
        return el.name.includes(tmpSearch);
      });
    },
    delDevice() {
      console.log("--delDevice--");
      console.log(this.selected);
      console.log(this.selected.length);
      for (var i = 0; i < this.selected.length; i++) {
        const item = this.selected[i];
        console.log(item.calories);
        // tinadel
        const url = "http://10.10.2.95:3000/api/v1/devices/" + item.calories;
        console.log(url);
        axios
          .delete(url)
          .then((response) => {
            console.log(response);
            //console.log("befoe del");
            this.clickGroup(this.currentGroup);
          })
          .catch((error) => console.log("addGroup error:" + error));
      }
    },
    addDevice() {
      const url = "http://172.23.70.74:3000/api/v1/devices";

      postFunc("/devices", {
        name: this.device_name,
        type: this.device_type,
        alias: this.device_alias,
      })
        .then((response) => {
          console.log(response);
          console.log("befoe change Device to group ");
          console.log(this.currentGroup);
          console.log(response.data.data.id);
          var devArr = [];
          devArr.push(response.data.data.id);
          postFunc("/groups/" + this.currentGroup + "/devices", {
            deviceId: devArr,
          })
            .then((response) => {
              //console.log(response);
              console.log("add dev to group OK");

              //this.getGroup();
            })
            .catch((error) => console.log("addDevice error:" + error));

          //this.getGroup();
          this.clickGroup(this.currentGroup);
        })
        .catch((error) => console.log("addDevice error:" + error));
    },
    alert() {
      // console.log("allllert");
      this.$q.notify({
        message: "Jim pinged you.",
        caption: "5 minutes ago",
        color: "secondary",
      });
    },
    getWord(word) {
      return this.$t(word);
    },
    clickDevice(val) {
      console.log(val);
    },
    deleteGroup(val) {
      console.log("deleteGroup");
      console.log(val);

      const url = "http://10.10.2.95:3000/api/v1/groups/" + val;
      console.log(url);
      // axios
      //   .delete(url)
      //   .then((response) => {
      //     console.log(response);
      //     console.log("befoe getGroup");
      //     this.getGroup();
      //   })
      //   .catch((error) => console.log("addGroup error:" + error));
    },
    async getGroup() {
      try {
        const response = await getFunc("/groups?pageSize=1000&page=0");

        this.contacts_list = [];
        console.log("===== mounted to method :");
        console.log(response);
        this.currentGroup;
        for (var prop in response.data.data.list) {
          var timestamp = response.data.data.list[prop].createdTime;
          var date = new Date(timestamp);

          var dateStr =
            date.getDate() +
            "/" +
            (date.getMonth() + 1) +
            "/" +
            date.getFullYear() +
            " " +
            date.getHours() +
            ":" +
            date.getMinutes() +
            ":" +
            date.getSeconds();

          const tmpObj = {
            name: response.data.data.list[prop].name,
            position: dateStr,
            avatar:
              "https://www.aetina.com/wp-content/uploads/solution/object_detection/feature_flexible_deployment.svg",
            id: response.data.data.list[prop].id,
          };
          this.contacts_list.push(tmpObj);
          if (prop == 0) {
            this.clickGroup(response.data.data.list[prop].id);
            this.selected_contact = tmpObj;
          }
        }
        this.clonedData = JSON.parse(JSON.stringify(this.contacts_list));

        if (response.data.ok) {
          console.log("icap-node data.ok");
        }
      } catch (error) {
        throw new Error(error);
      }
      //}.bind(this)

      // await axios
      //   .get("http://172.23.70.128:3000/api/v1/groups?pageSize=10&page=0")
      //   .then(
      //     function (response) {
      //       this.contacts_list = [];
      //       console.log("mounted to method :");
      //       console.log(response);
      //       for (var prop in response.data.data.list) {
      //         var timestamp = response.data.data.list[prop].createdTime;
      //         var date = new Date(timestamp);

      //         var dateStr =
      //           date.getDate() +
      //           "/" +
      //           (date.getMonth() + 1) +
      //           "/" +
      //           date.getFullYear() +
      //           " " +
      //           date.getHours() +
      //           ":" +
      //           date.getMinutes() +
      //           ":" +
      //           date.getSeconds();

      //         const tmpObj = {
      //           name: response.data.data.list[prop].name,
      //           position: dateStr,
      //           avatar:
      //             "https://www.aetina.com/wp-content/uploads/solution/object_detection/feature_flexible_deployment.svg",
      //           id: response.data.data.list[prop].id,
      //         };
      //         this.contacts_list.push(tmpObj);
      //         if (prop == 0) {
      //           this.clickGroup(response.data.data.list[prop].id);
      //           this.selected_contact = tmpObj;
      //         }
      //       }
      //       this.clonedData = JSON.parse(JSON.stringify(this.contacts_list));

      //       if (response.data.ok) {
      //         console.log("icap-node data.ok");
      //       }
      //     }.bind(this)
      //   );
    },
    async addGroup() {
      try {
        const group = await postFunc("/groups", {
          name: this.group_name,
        });
        console.log("==-----addGroup-----===");
        // console.log("finish post ---addGroup---")
        // console.table(group.data)
        await this.getGroup();
      } catch (error) {
        throw new Error(error);
      }
    },
    // async addGroup() {
    //   const url = "http://172.23.70.128:3000/api/v1/groups";

    //   axios
    //     .post(url, {
    //       name: this.group_name,
    //     })
    //     .then((response) => {
    //       console.log(response);
    //       console.log("befoe getGroup");
    //       this.getGroup();
    //     })
    //     .catch((error) => console.log("addGroup error:" + error));
    // },
    async clickGroup(value) {
      console.log("===clickGroup id===");
      console.log(value);
      this.currentGroup = value;
      //console.log(this.currentGroup);
      this.tableKey++;

      let devOnline = {};
      const devStatusUrl = "/devices/status?status=all";
      await getFunc(devStatusUrl).then(function (response) {
        // console.log("status?status=all");
        // console.log(response.data);
        // console.log(response.data.data.online);
        // console.log(response.data.data.online.length);
        // console.log(response.data.data.offline.length);
        // console.log("---status?status=all---");
        devOnline = response.data.data.online;
      });

      // const url =
      //   "http://10.10.2.75:3000/api/v1/groups/" + value + "/devices";
      // console.log(url);

      const url = "http://10.10.2.95:3000/api/v1/groups/" + value + "/devices";
      console.log(url);
      this.rows = [];

      const utl = "/groups/" + value + "/devices";
      const response1 = await getFunc(utl);
      console.log("response1");
      console.log(response1);

      for (var i = 0; i < response1.data.data.list.length; i++) {
        const getDevCreateTimeUrl =
          "/devices/" + response1.data.data.list[i].id + "/info";
        const response2 = await getFunc(getDevCreateTimeUrl);
        console.log("response2");
        console.log(response2);

        var date = new Date(response2.data.data.createdTime);
        var devStatus = "offline"
                var dateStr =
                  date.getDate() +
                  "/" +
                  (date.getMonth() + 1) +
                  "/" +
                  date.getFullYear() +
                  " " +
                  date.getHours() +
                  ":" +
                  date.getMinutes() +
                  ":" +
                  date.getSeconds();

                for (let i = 0; i < devOnline.length; i++) {
                  //console.log("=s=");
                  //console.log(devOnline[i].name);
                  //console.log(response2.data.data.name);
                  if (devOnline[i].id == response2.data.data.id) {
                    //response.data.data.id) {
                    devStatus = "online";
                    break;
                    //console.log(i);
                  } else {
                    devStatus = "offline";
                    //console.log(i);
                  }
                }


                const tmpObj = {
                  name: response2.data.data.name,
                  calories: response2.data.data.id, //"Jetson Nano",
                  fat: dateStr,
                  carbs: devStatus,
                  protein: 4,
                  sodium: 87,
                  calcium: "14%",
                  iron: "1%",
                };
                this.rows.push(tmpObj);
      }

      
      /*await axios.get(url).then(
        function (response) {
          console.log(response);

          var devStatus = "offline";
          for (var prop in response.data.data.list) {

            const getDevCreateTimeUrl =
              "/devices/" + response.data.data.list[prop].id + "/info";


            getFunc(getDevCreateTimeUrl).then(
              function (response) {


                var date = new Date(response.data.data.createdTime);

                var dateStr =
                  date.getDate() +
                  "/" +
                  (date.getMonth() + 1) +
                  "/" +
                  date.getFullYear() +
                  " " +
                  date.getHours() +
                  ":" +
                  date.getMinutes() +
                  ":" +
                  date.getSeconds();

                for (let i = 0; i < devOnline.length; i++) {
                  console.log("=s=");
                  console.log(devOnline[i].name);
                  console.log(response.data.data.name);
                  if (devOnline[i].id == response.data.data.id) {
                    //response.data.data.id) {
                    devStatus = "online";
                    //console.log(i);
                  } else {
                    devStatus = "offline";
                    //console.log(i);
                  }
                }


                const tmpObj = {
                  name: response.data.data.name,
                  calories: response.data.data.id, //"Jetson Nano",
                  fat: dateStr,
                  carbs: devStatus,
                  protein: 4,
                  sodium: 87,
                  calcium: "14%",
                  iron: "1%",
                };
                this.rows.push(tmpObj);
              }.bind(this)
            );
            console.log("this.rows");
            console.log(this.rows);

          }

          if (response.data.ok) {
            console.log("icap-node data.ok");
          }
        }.bind(this)
      );*/
    },
  },
  mounted() {
    const columns = [
      {
        name: "name",
        required: true,
        label: this.$t("name"), //this.$t("name"),//"Name",  () => this.$t("label")
        align: "left",
        field: (row) => row.name,
        format: (val) => `${val}`,
        sortable: true,
      },
      //{ name: "protein", label: "Alias", field: "protein", sortable: true },
      { name: "calories", label: "ID", field: "calories", sortable: true },
      { name: "fat", label: "Created Time", field: "fat", sortable: true },
      { name: "carbs", label: "Status", field: "carbs", sortable: true },
      // { name: "protein", label: "Alias", field: "protein", sortable: true },
      // { name: 'sodium', label: 'Sodium (mg)', field: 'sodium' },
      // { name: 'calcium', label: 'Calcium (%)', field: 'calcium', sortable: true, sort: (a, b) => parseInt(a, 10) - parseInt(b, 10) },
      // { name: 'iron', label: 'Iron (%)', field: 'iron', sortable: true, sort: (a, b) => parseInt(a, 10) - parseInt(b, 10) }
    ];

    this.getGroup();

    // this.time = setInterval(() => {
    //   // do something...
    //   this.alert();
    // }, 50000);
  },
});
</script>

<style scoped>
</style>
