<template lang="pug">
q-dialog(
  :value="visibility"
  minimized
  @hide="emitHideEvent()"
)
  q-card.card-info
    span.title-dialog Register Controller
    div.inputs-dialog.flex.column
      q-input(
        placeholder="Controller name"
        color="#0A5959"
        bg-color="$grey-2"
        v-model="infos.name"
        dense filled
        ).input-dialog
      q-input(
        placeholder="Controller token"
        color="#0A5959"
        bg-color="$grey-2"
        v-model="infos.token"
        dense filled
        ).input-dialog
      div.buttons-dialog.flex.column
        q-btn.register-dialog(
          @click="registerController()"
          ) Register
        q-btn.register-dialog(
          @click="emitHideEvent()"
          outline
          ) Cancel

</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import { connectControllers, getControllersInfo } from '../api/api'

export default {
  name: 'RegisterControllerComponent',
  props: {
    visibility: {
      type: Boolean,
      required: true
    }
  },
  data () {
    return {
      infos: {
        name: '',
        token: '',
        is_active: true
      },
      owner: ''
    }
  },
  methods: {
    ...mapActions('controllers', ['setUserControllers']),
    async emitHideEvent () {
      this.infos.name = ''
      this.infos.token = ''
      this.$emit('hide-dialog')
    },
    async registerController () {
      if (this.infos.name !== '' && this.infos.token !== '') {
        this.owner = this.currentUser.token
        await connectControllers({ ...this.infos }, this.currentUser.token)

        let userControllers
        userControllers = await getControllersInfo(this.currentUser)
        this.setUserControllers(userControllers)

        window.location.reload()
        this.emitHideEvent()
      } else {
        this.$q.notify({
          message: 'Controller name or controller token field is empty',
          color: 'negative'
        })
      }
    }
  },
  computed: {
    ...mapGetters('users', ['currentUser'])
  }
}
</script>
