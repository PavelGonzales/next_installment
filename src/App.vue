<template lang="pug">
  v-app(light)
    v-layout(column align-center justify-center)
      v-progress-circular(v-bind:size="290"
                          v-bind:width="20"
                          v-bind:rotate="270"
                          v-bind:value="percentSum"
                          color="teal") {{ saveMoney }}
      v-flex(xs12, sm6, offset-sm3)
    
        v-card
          v-dialog(v-model='dialog',
                   persistent, 
                   max-width='500px')
            v-btn(fab,
                  dark, 
                  absolute,
                  top,
                  right,
                  color='indigo',
                  slot='activator')
              v-icon(dark) add
            v-card
              v-card-title
                span.headline User Profile
              v-card-text
                v-container(grid-list-md)
                  v-layout(wrap)
                    v-flex(xs12, sm6, md4)
                      v-text-field(label='Legal first name', required)
                    
              v-card-actions
                v-spacer
                v-btn(color='blue darken-1', flat, @click.native='dialog = false') Close
                v-btn(color='blue darken-1', flat, @click.native='dialog = false') Save

          

          v-card-title(primary-title)
            div.elem Всего дней: {{ daysTotal || '-' }}
            div.elem Осталось дней: {{ daysCount || '-' }}
            div.elem В день: {{ perDay || '-' }}
            div.elem В месяц: {{ perMonth || '-' }}
            div.elem Уже есть: {{ saveMoney || '-' }}
            div.elem Осталось: {{ leftToSaveMoney || '-' }}
            div.elem В день с корректировкой: {{ perDayWidthAdjustment || '-' }}
            div.elem Следующий взнос: {{ nextInstallment || '-' }}


</template>

<script>
  import moment from 'moment'

  export default {
    data () {
      return {
        valid: false,
        start: false,
        end: false,
        date: {
          start: '2017-08-27',
          end: '2018-06-01'
        },
        sum: 200000,
        saveMoney: 41763,
        dialog: false
      }
    },
    computed: {
      moment: function () {
        return moment
      },
      daysTotal: function () {
        return moment(this.date.end).diff(moment(this.date.start), 'days')
      },
      daysCount: function () {
        return moment(this.date.end).diff(moment(), 'days')
      },
      perMonth: function () {
        return Math.ceil(this.sum / moment(this.date.end).diff(moment(this.date.start), 'month'))
      },
      perDay: function () {
        return Math.ceil(this.sum / this.daysTotal)
      },
      leftToSaveMoney: function () {
        return this.sum - this.saveMoney
      },
      perDayWidthAdjustment: function () {
        return Math.ceil(this.leftToSaveMoney / this.daysCount)
      },
      percentSum: function () {
        return this.saveMoney * 100 / this.sum
      },
      nextInstallment: function () {
        let i = 0
        let day
        let perDay
        let k

        while (true) {
          day = moment().add(i, 'day')
          k = moment(this.date.end).diff(moment(day), 'days')
          perDay = Math.ceil(this.leftToSaveMoney / k)
          if (perDay < this.perDay) {
            i++
          }
          if (perDay > this.perDay) {
            i--
          }
          if ((perDay >= this.perDay && perDay < this.perDay + 3) || (perDay <= this.perDay && perDay > this.perDay - 3)) {
            break
          }
        }
        return moment(day).format('DD.MM.YYYY')
      }
    }
  }
</script>

<style lang="stylus">
  @import './stylus/main'
  .progress-circular
    margin-top 25px
    margin-bottom 25px

  .form 
    padding 25px

  .elem 
    width 100%
    margin-bottom 7px

</style>
