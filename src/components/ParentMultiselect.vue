<!--
Description:
  Displays multiselect dropdown for already existing components in CreateComponent
  Functionality includes: selects parent for created component
  -->

<template>
  <div id="parent-select">
    <br />
    <multiselect
      v-model="value"
      placeholder="Parent Component"
      :multiple="false"
      :close-on-select="true"
      :options="options"
      @input="selectParent"
      @open="resetActiveComponent"
      :max-height="90"
      :option-height="20"
      :searchable="true"
    >
    <span slot='noResult'>No components found.</span>
    </multiselect>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'
import Multiselect from 'vue-multiselect'

export default {
  name: 'ParentMultiselect',
  components: {
    Multiselect
  },
  data () {
    return { value: '' }
  },
  computed: {
    ...mapState([
      'routes',
      'componentMap',
      'activeComponent',
      'activeRoute',
      'routes'
    ]),
    options () {
      return this.routes[this.activeRoute].map(component => component.componentName)
    }
  },
  methods: {
    ...mapActions(['parentSelected', 'setActiveComponent']),
    selectParent (value) {
      this.parentSelected(value)
    },
    // when multiselect is opened activeComponent is deselected to allow for parentSelected action
    resetActiveComponent () {
      if (this.activeComponent !== '') {
        this.setActiveComponent('')
      }
    }
  },
  // clears out value in mutiselect on creation of component
  watch: {
    componentMap: {
      handler () {
        // console.log('componentMap has changed')
        this.value = ''
      }
    }
  }
}
</script>

<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
<style scoped>
  #parent-select {
    height: 30px;
    margin: 0.75rem;
    width: 90%;
  }
</style>
