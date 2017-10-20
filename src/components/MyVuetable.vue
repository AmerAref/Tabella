<template>
  <div class="ui container">
    
    <filter-bar></filter-bar>
    <vuetable ref="vuetable"
      api-url="https://vuetable.ratiw.net/api/users"
      :fields="fields"
      pagination-path=""
      :per-page="20"
      :multi-sort="true"
      :sort-order="sortOrder"
      :append-params="moreParams"
      detail-row-component="my-detail-row"
      @vuetable:cell-clicked="onCellClicked"
      @vuetable:pagination-data="onPaginationData"
    >

    </vuetable>
    <div class="vuetable-pagination ui basic segment grid">
      <vuetable-pagination-info ref="paginationInfo"
      ></vuetable-pagination-info>
      <vuetable-pagination ref="pagination"
        @vuetable-pagination:change-page="onChangePage"
      ></vuetable-pagination>
    </div>
  </div>
</template>

<script>

import accounting from 'accounting'
import moment from 'moment'
import Vuetable from '/home/amer/amerone/node_modules/vuetable-2/src/components/Vuetable'
import VuetablePagination from '/home/amer/amerone/node_modules/vuetable-2/src/components/VuetablePagination'
import VuetablePaginationInfo from '/home/amer/amerone/node_modules/vuetable-2/src/components/VuetablePaginationInfo'
import FieldDefs from './FieldDefs.js'
import DetailRow from './DetailRow'
import FilterBar from './FilterBar'
import Vue from 'vue'
import VueEvents from 'vue-events'

Vue.use(VueEvents)
Vue.component('my-detail-row', DetailRow)
Vue.component('filter-bar', FilterBar)

export default {
  components: {
    Vuetable,
    VuetablePagination,
    VuetablePaginationInfo
  },
  data () {
    return {
      fields: FieldDefs,
      sortOrder: [
        {
          field: 'email',
          sortField: 'email',
          direction: 'asc'
        }
      ],
      moreParams: {}
    }
  },
  mounted () {
    this.$events.$on('filter-set', eventData => this.onFilterSet(eventData))
    this.$events.$on('filter-reset', e => this.onFilterReset())
  },
  methods: {
    allcap (value) {
      return value.toUpperCase()
    },
    formatNumber (value) {
      return accounting.formatNumber(value, 2)
    },
    formatDate (value, fmt = 'D MMM YYYY') {
      return (value == null)
        ? ''
        : moment(value, 'YYYY-MM-DD').format(fmt)
    },
    onPaginationData (paginationData) {
      this.$refs.pagination.setPaginationData(paginationData)
      this.$refs.paginationInfo.setPaginationData(paginationData)
    },
    onChangePage (page) {
      this.$refs.vuetable.changePage(page)
    },
    onCellClicked (data, field, event) {
      console.log('cellClicked: ', field.name)
      this.$refs.vuetable.toggleDetailRow(data.id)
    },
    onFilterSet (filterText) {
      this.moreParams = {
        'filter': filterText
      }
      Vue.nextTick(() => this.$refs.vuetable.refresh())
    },
    onFilterReset () {
      this.moreParams = {}
      Vue.nextTick(() => this.$refs.vuetable.refresh())
    }
  }
}
</script>


<style>
.vuetable-pagination {
  background: yellowgreen  !important ;
}

.vuetable-pagination-info{
    background: yellowgreen  !important ;
}

</style>