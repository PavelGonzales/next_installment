<template lang="pug">
  v-app(light)
    v-navigation-drawer(persistent, 
                        v-model='drawer', 
                        enable-resize-watcher, 
                        app)
      v-list(dense)
        v-list-tile
          v-flex
            v-avatar(size='36px', slot='activator')
              img(src='https://pp.userapi.com/c840639/v840639432/135da/a-vQaRxBd3s.jpg', alt='')
            strong Pavel Gonzales

        v-list-tile(@click="")
          .tile-item
            v-flex
              strong На тачку
              v-progress-linear(v-model="percentSum")

        v-list-tile(@click="")
          v-list-tile-action
            v-icon add
          v-list-tile-content
            v-list-tile-title New 

    v-toolbar(color='cyan', dark)
      v-toolbar-side-icon(@click.stop="drawer = !drawer")
      v-toolbar-title Next Installment
      v-spacer
      v-btn(icon)
        v-icon more_vert

    v-layout(column align-center justify-center)
      v-progress-circular(v-bind:size="260"
                          v-bind:width="20"
                          v-bind:rotate="270"
                          v-bind:value="percentSum"
                          color="teal") 
        div {{ saveMoney }}
        hr 
        div {{ leftToSaveMoney }}
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
                span.headline Добавить
              v-card-text
                v-container(grid-list-md)
                  v-layout(wrap)
                    v-flex
                      v-menu(lazy
                             v-model='end', 
                             transition='scale-transition', 
                             offset-y, 
                             full-width, 
                             :nudge-right='40', 
                             max-width='290px', 
                             min-width='290px')
                        v-text-field(slot='activator', 
                                    label='Дата', 
                                    v-model='sum.add', 
                                    prepend-icon='event', 
                                    readonly)
                        v-date-picker(v-model='date.add', 
                                      no-title, 
                                      scrollable, 
                                      actions)
                          template(slot-scope='{ save, cancel }')
                      v-text-field(name="sum"
                                  v-model="addMoney"
                                  type="number"
                                  prepend-icon='attach_money',
                                  label="Сумма")
                    
              v-card-actions
                v-spacer
                v-btn(color='blue darken-1', 
                      flat, 
                      @click.native='dialog = false') Close
                v-btn(color='blue darken-1', 
                      flat, 
                      @click.native='dialog = false'
                      @click="save()") Save

          

          v-card-title(primary-title)
            div.elem Дней: {{ daysTotal - daysCount || '-' }} / {{ daysTotal || '-' }}
              v-progress-linear(v-model="valueDeterminate")

            div.elem
              v-chip(color='green', text-color='white')
                v-avatar.green.darken-4 {{ perDay || '-' }}
                | В день

              v-chip(color='green', text-color='white')
                v-avatar.green.darken-4 {{ perMonth || '-' }}
                | В месяц
                
              v-chip(color='green', text-color='white')
                v-avatar.green.darken-4 {{ perDayWidthAdjustment || '-' }}
                | В день с корректировкой

            div.elem Рекомендуемый следующий взнос: 
              strong {{ nextInstallment || '-' }}


</template>

<script>
  import moment from 'moment'

  export default {
    data () {
      return {
        drawer: true,
        valid: false,
        start: false,
        end: false,
        add: false,
        date: {
          start: '2017-08-27',
          end: '2018-06-01',
          add: moment().format('YYYY-MM-DD')
        },
        sum: 200000,
        saveMoney: 41763,
        addMoney: 0,
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
      valueDeterminate: function () {
        return (this.daysTotal - this.daysCount) * 100 / this.daysTotal
      },
      nextInstallment: function () {
        if (this.saveMoney > this.sum) return 'Done'
        let i = 0
        let day
        let perDay

        while (true) {
          day = moment().add(i, 'day')
          perDay = Math.ceil(this.leftToSaveMoney / moment(this.date.end).diff(moment(day), 'days'))
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
    },
    methods: {
      save: function () {
        console.log('date.add', Number(this.addMoney))
        this.saveMoney = this.saveMoney + Number(this.addMoney)
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

  .tile-item
    width 100%
    margin-top 40px
    margin-bottom 15px
    .progress-linear
      margin-top 3px


</style>
