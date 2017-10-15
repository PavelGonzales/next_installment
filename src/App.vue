<template lang="pug">
  v-app(light)
    v-layout
      v-flex(xs12, sm6, offset-sm3)
        v-card
          v-form.form
            v-menu(lazy
                   v-model='start', 
                   transition='scale-transition', 
                   offset-y, 
                   full-width, 
                   :nudge-right='40', 
                   max-width='290px', 
                   min-width='290px')
              v-text-field(slot='activator', 
                          label='Дата старта', 
                          v-model='date.start', 
                          prepend-icon='event', 
                          readonly)
              v-date-picker(v-model='date.start', 
                            no-title, 
                            scrollable, 
                            actions)
                template(slot-scope='{ save, cancel }')

            v-menu(lazy
                   v-model='end', 
                   transition='scale-transition', 
                   offset-y, 
                   full-width, 
                   :nudge-right='40', 
                   max-width='290px', 
                   min-width='290px')
              v-text-field(slot='activator', 
                          label='Дата конца', 
                          v-model='date.end', 
                          prepend-icon='event', 
                          readonly)
              v-date-picker(v-model='date.end', 
                            no-title, 
                            scrollable, 
                            actions)
                template(slot-scope='{ save, cancel }')
            v-text-field(name="sum"
                         prepend-icon='attach_money',
                         label="Сумма")

          v-dialog(v-model='dialog',
                   persistent, 
                   max-width='500px')
            v-btn(fab,
                  dark, 
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
            div.elem Всего дней: -
            div.elem Осталось дней: -
            div.elem В день: -
            div.elem В месяц: -
            div.elem Уже есть: -
            div.elem Осталось: -
            div.elem В день с корректировкой: -
            div.elem Следующий взнос: -


</template>

<script>
  export default {
    data () {
      return {
        valid: false,
        start: false,
        end: false,
        date: {
          start: null,
          end: null
        },
        sum: '',
        dialog: false
      }
    }
  }
</script>

<style lang="stylus">
  @import './stylus/main'

  .form 
    padding 25px

  .elem 
    width 100%
    margin-bottom 7px

</style>
