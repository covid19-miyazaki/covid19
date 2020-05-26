<template>
  <div class="MainPage">
    <div class="Header mb-3">
      <page-header :icon="headerItem.icon">
        <t-i18n>{{ headerItem.title }}</t-i18n>
      </page-header>
      <div class="UpdatedAt">
        <t-i18n>{{ $t('最終更新') }}</t-i18n>
        <time :datetime="updatedAt">{{ Data.lastUpdate }}</time>
      </div>
      <div v-if="!['ja', 'ja-basic'].includes($i18n.locale)" class="Annotation">
        <t-i18n>{{ $t('注釈') }}</t-i18n>
      </div>
    </div>
    <static-info
      class="mb-4"
      :url="
        'https://www.pref.miyazaki.lg.jp/kansensho-taisaku/kenko/hoken/kinkyujitaisengen_covid19.html'
      "
      :text="
        $t(
          '「緊急事態宣言」が全国一律で5月末まで延長され、また国の基本的対処方針が改定されました'
        )
      "
    />
    <whats-new class="mb-4" :items="newsItems" />
    <static-info
      class="mb-4"
      :url="
        'https://www.pref.miyazaki.lg.jp/kansensho-taisaku/kenko/hoken/20200121143156.html#center'
      "
      :text="$t('自分や家族の症状に不安や心配があればまずは電話相談をどうぞ')"
      :btn-text="$t('相談の手順を見る')"
    />
    <v-row class="DataBlock">
      <confirmed-cases-details-card />
      <confirmed-cases-number-card />
      <confirmed-cases-attributes-card />
    </v-row>
    <v-divider />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { MetaInfo } from 'vue-meta'
import PageHeader from '@/components/PageHeader.vue'
import WhatsNew from '@/components/WhatsNew.vue'
import StaticInfo from '@/components/StaticInfo.vue'
import Data from '@/data/data.json'
import News from '@/data/news.json'
import ConfirmedCasesDetailsCard from '@/components/cards/ConfirmedCasesDetailsCard.vue'
import ConfirmedCasesNumberCard from '@/components/cards/ConfirmedCasesNumberCard.vue'
import ConfirmedCasesAttributesCard from '@/components/cards/ConfirmedCasesAttributesCard.vue'
import { convertDatetimeToISO8601Format } from '@/utils/formatDate'
import TI18n from '@/components/TI18n.vue'

export default Vue.extend({
  components: {
    PageHeader,
    WhatsNew,
    StaticInfo,
    ConfirmedCasesDetailsCard,
    ConfirmedCasesNumberCard,
    ConfirmedCasesAttributesCard,
    TI18n
  },
  data() {
    const data = {
      Data,
      headerItem: {
        icon: 'mdi-chart-timeline-variant',
        title: this.$t('宮崎県内の最新感染動向')
      },
      newsItems: News.newsItems
    }
    return data
  },
  computed: {
    updatedAt() {
      return convertDatetimeToISO8601Format(this.$data.Data.lastUpdate)
    }
  },
  head(): MetaInfo {
    return {
      title: this.$t('宮崎県内の最新感染動向') as string
    }
  }
})
</script>

<style lang="scss" scoped>
.MainPage {
  .Header {
    display: flex;
    flex-wrap: wrap;
    align-items: flex-end;

    @include lessThan($small) {
      flex-direction: column;
      align-items: baseline;
    }
  }

  .UpdatedAt {
    @include font-size(14);

    color: $gray-3;
    margin-bottom: 0.2rem;
  }

  .Annotation {
    @include font-size(12);

    color: $gray-3;
    @include largerThan($small) {
      margin: 0 0 0 auto;
    }
  }
  .DataBlock {
    margin: 20px -8px;

    .DataCard {
      @include largerThan($medium) {
        padding: 10px;
      }

      @include lessThan($small) {
        padding: 4px 8px;
      }
    }
  }
}
</style>
