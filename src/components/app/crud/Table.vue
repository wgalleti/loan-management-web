<template>
  <div id="lista">

      <v-toolbar flat dark color="black">
        <v-text-field
            v-model="search"
            append-icon="search"
            placeholder="Search"
            single-line
            hide-details
            class="mb-2"
            dark
          >
          </v-text-field>
        <v-spacer></v-spacer>
        <v-btn icon @click="$emit('reloadItems')" :loading="loading">
          <v-icon medium>refresh</v-icon>
        </v-btn>

      </v-toolbar>
      <v-flex xs12>
        <v-data-table
          :headers="headers"
          :items="data"
          :search="search"
          :loading="loading"
        >
          <template v-slot:items="props" v-if="!loading">
            <tr @click="$emit('selectedRow', props.item)">
              <td
                v-for="i in headers.filter(h => h.value)"
                :key="i.value"
                :class="{'text-xs-right': i.align === 'right'}"
              >
                {{props.item[i.value]}}
              </td>
              <template v-if="headers.filter(h => !h.value).length > 0">
                <td class="justify-center layout ma-1">
                  <v-icon
                    small
                    class="mr-1"
                    @click="$emit('editItem', props.item)"
                  >
                    edit
                  </v-icon>
                  <app-crud-remove :item="props.item"
                              :apiRemove="apiRemove"
                              @removeItem="$emit('reloadItems')">
                  </app-crud-remove>
                </td>
              </template>
            </tr>
          </template>
          <template v-slot:footer v-else>
            <td :colspan="headers.length" class="text-xs-center">
              <v-progress-circular
                color="primary darken-2"
                class="ma-3"
                indeterminate
                :size="70"
                :width="7"
              ></v-progress-circular>
            </td>
          </template>
        </v-data-table>
      </v-flex>
  </div>
</template>

<script>

export default {
  props: {
    title: {
      type: String
    },
    headers: {
      type: Array,
      required: true
    },
    data: {
      required: true
    },
    loading: {
      type: Boolean,
      default: false
    },
    apiRemove: {
      type: String
    }
  },
  data () {
    return {
      search: ''
    }
  }
}
</script>
