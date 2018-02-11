<template>
  <div>
    <v-container>
        <v-layout>
          <v-flex xs12>
            <v-card color="purple">
              <v-container fluid grid-list-lg>
                <v-layout row>
                  <v-flex xs5>
                    <v-card-media
                      :src="user.urlimage"
                      height="125px"
                      contain
                    ></v-card-media>
                  </v-flex>
                  <v-flex xs7>
                    <div>
                      <div>{{user.fullname}}</div>
                      <div>{{user.birthday}}</div>
                      <div>{{user.mobile}}</div>
                      <div>{{user.email}}</div>
                      <div>{{user.kong}}</div>
                      <div>{{user.samnak}}</div>
                      <div>
                        <qrcode-vue :value="user.id" size="150" level="H"></qrcode-vue>
                      </div>
                    </div>
                  </v-flex>
                </v-layout>
              </v-container>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
  </div>
</template>

<script>
  import QrcodeVue from 'qrcode.vue'

  export default {
    computed: {
      user () {
        return this.$store.getters.user
      },
      loading () {
        return this.$store.getters.loading
      }
    },
    methods: {
      onLoadMeetup (id) {
        // this.$router.push('/meetups/' + id)
      }
    },
    components: {
      QrcodeVue
    }
  }
</script>
