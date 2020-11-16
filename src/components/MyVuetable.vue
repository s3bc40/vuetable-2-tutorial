<template>
    <div class="ui container">
        <filter-bar></filter-bar>
        <!-- <div class="vuetable-pagination ui basic segment grid">
            <vuetable-pagination-info ref="paginationInfoTop"
            ></vuetable-pagination-info>
            <vuetable-pagination ref="paginationTop"
                @vuetable-pagination:change-page="onChangePage"
            ></vuetable-pagination>
        </div> -->
        <vuetable ref="vuetable"
            :multi-sort="true"
            :sort-order="sortOrder"
            :api-url="apiUrl"
            :fields="fields"
            :per-page="10"
            :append-params="appendParams"
            @vuetable:pagination-data="onPaginationData"
            @vuetable:cell-clicked="onCellClicked"
            pagination-path=""
            detail-row-component="detailRowComponent"
        ></vuetable>
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
import Vue from 'vue'
import VueEvents from 'vue-events'
import accounting from 'accounting'
import moment from 'moment'
import Vuetable from 'vuetable-2/src/components/Vuetable'
import VuetablePagination from 'vuetable-2/src/components/VuetablePagination'
// import VuetablePagination from 'vuetable-2/src/components/VuetablePaginationDropdown'
import VuetablePaginationInfo from 'vuetable-2/src/components/VuetablePaginationInfo'
import CustomActions from './CustomActions'
import FilterBar from './FilterBar'

Vue.use(VueEvents)

Vue.component('custom-actions', CustomActions)
Vue.component('filter-bar', FilterBar)

export default {
    components: {
        Vuetable,
        VuetablePagination,
        VuetablePaginationInfo,
    },
    props: {
        apiUrl: {
            type: String,
            required: true
        },
        fields: {
            type: Array,
            required: true
        },
        sortOrder: {
            type: Array,
            default() {
                return []
            }
        },
        appendParams: {
            type: Object,
            default() {
                return {}
            }
        },
        detailRowComponent: {
            type:String
        }
    },
    data () {
        return {}
    },
    mounted () {
        this.$events.$on('filter-set', eventData => this.onFilterSet(eventData))
        this.$events.$on('filter-reset', e => this.onFilterReset())
    },
    methods: {
        allcap (value) {
            return value.toUpperCase()
        },
        genderLabel (value) {
            return value === 'M'
            ? '<span class="ui teal label"><i class="large man icon"></i>Male</span>'
            : '<span class="ui pink label"><i class="large woman icon"></i>Female</span>'
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
            // this.$refs.paginationTop.setPaginationData(paginationData)
            // this.$refs.paginationInfoTop.setPaginationData(paginationData)

            this.$refs.pagination.setPaginationData(paginationData)
            this.$refs.paginationInfo.setPaginationData(paginationData)
        },
        onChangePage (page) {
            this.$refs.vuetable.changePage(page)
        },
        onCellClicked (data, field, envent) {
            console.log(`CellClicked: ${field.name}`)
            this.$refs.vuetable.toggleDetailRow(data.id)
        },
        onFilterSet (filterText) {
            this.appendParams.filter = filterText
            Vue.nextTick( () => this.$refs.vuetable.refresh())
        },
        onFilterReset () {
            delete appendParams.filter
            Vue.nextTick( () => this.$refs.vuetable.refresh())
        }
    }
}
</script>