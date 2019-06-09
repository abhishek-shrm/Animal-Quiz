<template>
  <div>
    <b-nav tabs><!-- tabs is used here for off setting the header -->
    <b-nav-item disabled><b>Fancy Quiz App</b></b-nav-item>
    <b-nav-item disabled>Counter: {{numCorrect}}/{{numTotal}}</b-nav-item>
  </b-nav>
  </div>
</template>

<script>
export default {
  props:[
    'numCorrect',
    'numTotal'
  ]
}
</script>