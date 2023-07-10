

<script>
import habitData from "@/components/HabitData.vue";

export default {
  name: 'HabitCard',
  computed: {
    habitData() {
      return habitData
    }
  },
  props: {
    habit: {
      type: Object,
      required: true
    }
  },
  methods: {
    getAvatar (habit) {
      if (habit.charge === 'positive') {
        return require('../assets/pos.png')
      } else if (habit.charge === 'negative') {
        return require('../assets/neg.png')
      }else return require('../assets/neu.png')
    }
  }
}
</script>

<style scoped>

</style>