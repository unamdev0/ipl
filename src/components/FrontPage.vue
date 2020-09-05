<template>
    <div>
        Hi welcome to the quiz
        <button>Play</button>
        <button>HighScore {{highscore}}</button>
    </div>
</template>


<script>
export default {
    name:'FrontPage',
    props:['highscore']
    
    
}
</script>

<style bound>

</style>