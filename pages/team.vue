<template>
  <v-container>
    <v-layout wrap>
      <v-flex xs10 sm6 offset-sm3 offset-xs1>
        <v-card class="mt-0 mb-4" color="#C1FFC1">
          <v-card-text>
            <div>
              <h1 class="team-header">
                Team
              </h1>
            </div>
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
    <PageLoader v-show="!members.length" />
    <v-layout row wrap>
      <v-card
        v-for="member in members"
        :key="member['.key']"
        class="mx-auto mb-3"
        width="400"
      >
        <v-flex xs12 class="text-xs-center">
          <v-avatar :tile="false" :size="200" color="grey lighten-4 mt-3">
            <v-img
              :aspect-ratio="16 / 9"
              :src="member.thumbnail"
              alt="Avatar"
            />
          </v-avatar>
        </v-flex>
        <v-card-title>
          <v-flex xs12 class="text-xs-center">
            <div>
              <span class="headline">{{ member.name }}</span>
              <div class="d-flex">
                <div class="ml-2 grey--text text--darken-2">
                  <span>{{ member.position }}</span>
                </div>
              </div>
            </div>
          </v-flex>
        </v-card-title>
        <v-flex class="grey lighten-3 text-xs-center">
          <v-btn
            v-for="(link, type) in member.links"
            :key="type"
            :color="iconColor(type)"
            class="mx-3"
            fab
            dark
            small
          >
            <a
              :href="getLink(link, type)"
              target="_blank"
              style="text-decoration: none; color: inherit"
            >
              <v-icon dark>{{ icon(type) }}</v-icon>
            </a>
          </v-btn>
        </v-flex>
      </v-card>
    </v-layout>
  </v-container>
</template>

<script>
import { mapActions, mapGetters } from 'vuex'
import PageLoader from '@/components/widgets/PageLoader.vue'

export default {
  head() {
    return {
      title: 'Team'
    }
  },
  components: { PageLoader },
  computed: {
    ...mapGetters('team', ['members'])
  },
  created() {
    this.setTeamRef()
  },
  methods: {
    iconColor(type) {
      switch (type) {
        case 'facebook':
          return 'blue darken-4'
        case 'email':
          return 'red darken-1'
        case 'g-plus':
          return 'red darken-4'
        case 'linkedin':
          return 'indigo darken-1'
        case 'mobile':
          return 'green darken-3'
        case 'twitter':
          return 'light-blue darken-1'
      }
    },
    icon(type) {
      switch (type) {
        case 'g-plus':
          return 'mdi-google-plus'
        case 'mobile':
          return 'mdi-phone'
        default:
          return `mdi-${type}`
      }
    },
    getLink(link, type) {
      if (type === 'email') {
        return 'mailto:' + link
      } else if (type === 'mobile') {
        return 'tel:' + link
      } else if (!link.startsWith('http://') && !link.startsWith('https://')) {
        return '//' + link
      }

      return link
    },
    ...mapActions('team', ['setTeamRef'])
  }
}
</script>

<style scoped>
.team-header {
  color: black;
  text-align: center;
}
.contact {
  font-size: 35px;
}
</style>
