<template>
  <div>
    <template v-if="contest">
      <v-container class="pa-0" fluid>
        <v-card>
          <v-card-title>すべての提出</v-card-title>
          <v-card-text style="color:inherit">
            <SubmitTable v-if="submits" :submits="submits" />
          </v-card-text>
        </v-card>
      </v-container>
    </template>
  </div>
</template>

<script lang="ts">
import { Component, mixins } from 'nuxt-property-decorator'
import ContestHeader from '~/components/ContestHeader.vue'
import ContestHeaderTab from '~/components/ContestHeaderTab.vue'
import MathJax from '~/mixins/mathjax'
import MixinContest from '~/mixins/contest'
import SubmitTable from '~/components/submits/SubmitTable.vue'
import { Submit } from '~/types/submits'
@Component({
  components: { SubmitTable, ContestHeaderTab, ContestHeader },
  layout: 'contest'
})
export default class PageContest extends mixins(MathJax, MixinContest) {
  async created() {
    await this.getContest()
    this.$api.Contests.allSubmits(this.$route.params.contestName).then(
      (res: Submit[]) => {
        this.submits = res
      }
    )
  }

  submits: Submit[] | null = null
}
</script>

<style lang="scss" scoped></style>
