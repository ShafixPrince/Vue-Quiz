<template>

  <div>
    <b-nav tabs>
      <b-nav-item disabled> <b> Fancy Quiz app </b> </b-nav-item>
      <b-nav-item disabled>Counter: {{ numCorrect }}/{{ numTotal }}</b-nav-item>

    </b-nav>
  </div>

</template>

<script>

export default{


  props: [   //props is to travesre data between the app vue to the header vue 
    'numCorrect',
    'numTotal'
  ]
}

</script>
