<template>
  <div>
    <div class="q-pa-md">
      <q-table grid grid-header title="Followers" :rows="entries" :columns="columns"
        row-key="uid" :filter="filter" :rows-per-page-options="[0]"
        class="my-sticky-column-table">
        <template v-slot:top-right>
          <q-input rounded outlined dense debounce="300" v-model="filter" placeholder="Search">
            <template v-slot:append>
              <q-icon name="search" />
            </template>
          </q-input>
        </template>

        <template v-slot:item="props">
          <div class="q-pa-md q-pt-md col-xs-12 col-sm-6 col-md-3">
            <q-card class="my-card">
              <q-img :src="props.row.image" />
              <q-card-section>
                <div class="row col no-wrap items-center">
                  <div class="col-10 text-h6">{{props.row.fullname}}</div>
                  <div class="col text-h6 text-right">{{props.row.twubric.total}}</div>
                </div>
              </q-card-section>
              <q-separator />
              <q-card-section>
                <div class="row col no-wrap items-center">
                  <div class="col-4 text-center">{{props.row.twubric.friends}}</div>
                  <div class="col-4 text-center">{{props.row.twubric.influence}}</div>
                  <div class="col-4 text-center">{{props.row.twubric.chirpiness}}</div>
                </div>
                <div class="row col no-wrap items-center">
                  <div class="col-4 text-center">Friends</div>
                  <div class="col-4 text-center">Influence</div>
                  <div class="col-4 text-center">Chirpiness</div>
                </div>
              </q-card-section>
              <q-separator />
              <q-card-actions class="full-width block">
                <div class="col row">
                  <div class="col-6 self-center">{{props.row.join_date}}</div>
                  <div class="col-6">
                    <q-btn round flat icon="delete" class="float-right q-mr-xs"
                    @click="deleteItem(props.row.uid)"/>
                  </div>
                </div>
              </q-card-actions>
            </q-card>
          </div>
        </template>
      </q-table>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import axios from 'axios';
/* eslint-disable */
export default {
  el: '#app',
  data() {
    const columns = [
      {
        name: 'name', label: 'Name', field: 'fullname', align: 'center', sortable: true,
      },
      {
        name: 'total', label: 'Twubric Score', field: row => row.twubric.total, align: 'center', sortable: true,
      },
      {
        name: 'friends', label: 'Friends', field: row => row.twubric.friends, align: 'center', sortable: true,
      },
      {
        name: 'influence', label: 'Influence', field: row => row.twubric.influence, align: 'center', sortable: true,
      },
      {
        name: 'chirpiness', label: 'Chirpiness', field: row => row.twubric.chirpiness, align: 'center', sortable: true,
      },
    ];
    const entries = ref([]);

    axios.get('https://gist.githubusercontent.com/pandemonia/21703a6a303e0487a73b2610c8db41ab/raw/82e3ef99cde5b6e313922a5ccce7f38e17f790ac/twubric.json')
      .then((response) => {
        entries.value = response.data;
      });
    return {
      columns,
      filter: ref(''),
      entries,
    };
  },
  methods: {
    deleteItem(id) {
      const newArray = this.entries.filter((card) => card.uid !== id);
      this.entries = [...newArray];
    },
  },
};
</script>
<style lang="scss">
.grid-masonry {
  flex-direction: column;
  height: 700px;
}
.flex {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}
.my-sticky-column-table {
  height: 78vh;

  tr th {
    position: sticky;
    z-index: 2;
    background: #e4e4e4;
  }
}
.q-table {
  margin-top: 4px;
  border: 1px solid rgb(53, 51, 51);
}
.q-table__grid-content {
  margin-top: 3rem;
}
body {
  background: #e4e4e4;
}
</style>
