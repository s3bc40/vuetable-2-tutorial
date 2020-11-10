<template>
    <vuetable ref="vuetable"
        api-url="https://vuetable.ratiw.net/api/users"
        :fields="fields"
    ></vuetable>
</template>

<script>
import Vuetable from 'vuetable-2/src/components/Vuetable'
import accounting from 'accounting'
import moment from 'moment'
// https://github.com/ratiw/vuetable-2-tutorial/wiki/lesson-06 WIP
export default {
    components: {
        Vuetable
    },
    data () {
        return {
            fields: [
                'name', 'email',
                {
                    name: 'birthdate',
                    titleClass: 'center aligned',
                    dataClass: 'center aligned',
                },
                {
                    name: 'nickname',
                    callback: 'allcap',
                },
                {
                    name: 'gender',
                    titleClass: 'center aligned',
                    dataClass: 'center aligned',
                    callback: 'genderLabel',
                },
                {
                    name: 'salary',
                    titleClass: 'center aligned',
                    dataClass: 'right aligned',
                    callback: 'formatNumber',
                },
            ]
        }
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
        }
    }
}
</script>