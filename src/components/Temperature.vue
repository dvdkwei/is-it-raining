<script>/* eslint-disable */
import {h} from "vue";
import Current from './Current.vue';

export default {
  name: "Temperature",
  props: {
    val: {
      default: 0
    },
    degree: {
      default: ''
    }
  },
  setup({val, degree}) {
    return () => h('div',
        {
          class: 'temperature',
          style: {
            display: 'inline-flex',
            justifyContent: 'center',
            alignItems: 'center',
            height: '150px'
          }
        },
        [
          h('p',
              {
                class: 'value',
                style: {
                  fontSize: '150px',
                  margin: '0'
                }
              },
              val),
          h('p',
              {
                style: {
                  fontSize: '50px',
                  margin: '0',
                  alignSelf: 'end'
                }
              },
              ' °' + degree
          )
        ]
    )
  }
}
</script>

<style scoped/>