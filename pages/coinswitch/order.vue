<template lang="pug">
.row.mt-3
  .col
    .lead {{ $t('Exchanging 12 ETH to 7234 EOS') }}

    .row.mt-3
      .col
        el-steps(:space='200' :active='step' simple)
          el-step(:title='$t("Enter Address")' icon='el-icon-edit' :description="$t('Some description')")
          el-step(:title='$t("Send Coin")' icon='el-icon-upload')
          el-step(:title='$t("Exchange Status")' icon='el-icon-picture')

    .row(v-if="step == 0").mt-4
      .col-8
        el-card
          h4 {{ $t('Enter your EOS wallet address') }}
          span {{ $t('Address for the wallet in which EOS will be deposited') }}

          .row.mt-3
            .col
              el-input(v-model="walletAddress" :placeholder="$t('Enter your EOS wallet address')" clearable)

          .row.mt-3
            .col
              .row
                .col
                  el-button(type="text") {{ $t("Don't have wallet yet?") }}
              el-button(type="primary" @click="createOrder" v-loading="loading").w-100 {{ $t('Next') }}

      .col-4
        el-card.box-card.h-100
          .test-center(slot="header" class="clearfix")
            .lead {{ $t('EOS ORDERS') }}

          .row
            .col-2.p-3.text-center
              .lead.el-icon-time
            .col-6
              b {{ $t('6 Minutes') }}
              div {{ $t('avg completion time') }}

          .row.mt-2
            .col-2.p-3.text-center
              .lead.el-icon-trophy-1
            .col-6
              b {{ $t('21% faster') }}
              div {{ $t('than other exchanges') }}

    .row(v-if="step == 1")
      .col

    .row(v-if="step == 2")
      .col

</template>

<script>
export default {
  data() {
    return {
      loading: false,

      step: 0,

      walletAddress: '' // Add check
    }
  },

  watch: {
  },

  mounted() {
  },

  methods: {
    async createOrder() {
      this.loading = true

      try {
        const { data } = await this.$axios.post('coinswitch/order', {
          depositCoin: 'eth',
          destinationCoin: 'eos',
          depositCoinAmount: 12.0, // TODO Тут лучше сделать дестинейшн
          destinationAddress: this.walletAddress
        })

        if (!data.success) throw new Error(data.msg)

        console.log(data)
        this.step = 1
      } catch (e) {
        this.$notify({ title: 'Order creation', message: e, type: 'error' })
      } finally {
        this.loading = false
      }
    }
  }
}
</script>

<style lang="scss">
</style>
