<template lang="pug">
  div
    transition(name="fade")
      #darken(v-if="navActive")

    header.header
      .flex-left
        a#sidebarBtn.flex-left(@click="toggleSidebar")
          i.fa.fa-bars

      .flex-right.flex
        .welcome
          | Hi, 
          span.username {{ display_name }}
          | !

        img.user-img(
          :src="profile_picture"
        )
</template>

<script>
export default {
  name: 'HeaderItem',
  data() {
    return {
      display_name: '',
      profile_picture: '',
    }
  },
  computed: {
    navActive() {
      return this.$store.state.navActive
    },
  },
  created() {
    this.$http.get('/api/get_user_info')
      .then(response => {
        const data = response.data;
        if (data.result === 'success') {
          this.display_name = data.data.display_name,
          this.profile_picture = data.data.profile_picture
        }
      })
      .catch(error => console.log(error))
  },
  methods: {
    toggleSidebar() {
      this.$store.commit('toggleNav');
    },
  },
}
</script>

<style scoped lang="stylus">
.header
  background white
  box-shadow 0 0 8px 2px rgba(0, 0, 0, .1)
  color #60727b
  display flex
  left 250px
  padding 15px 25px
  position fixed
  right 0
  top 0
  z-index 2

  *
    align-self center
    margin auto

  @media (max-width: 768px)
    left 0
    padding 10px 25px

$mobile-display
  display none

  @media (max-width: 768px)
    display inline

#darken
  @extend $mobile-display
  background rgba(0, 0, 0, .3)
  bottom 0
  left 0
  right 0
  position fixed
  top 0
  z-index 1

#sidebarBtn
  @extend $mobile-display
  font-size 1.4em

.flex
  display flex

.flex-left
  margin-left 0

.flex-right
  margin-right 0

.welcome
  padding-right 7px

.username
  color #6956c1
  font-weight bold

.user-img
  background lightgrey
  border-radius 50%
  height 32px
  width @height

.fade-enter-active
.fade-leave-active
  transition opacity .3s

.fade-enter
.fade-leave-to
  opacity 0
</style>