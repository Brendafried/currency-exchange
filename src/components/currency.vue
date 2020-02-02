<template>
  <div class='currency__container'>
    <div class='currency__code'>{{code}}</div>
    <div class='currency__text'>{{text}}</div>
    <div class='currency__country'>{{country}}</div>
  </div>
</template>

<script>
export default {
  name: 'currency',
  props: {
    code: {
      type: String
    },
    text: {
      type: String
    },
    country: {
      type: String
    }

  }
}
</script>
<style>
.currency__container {
  width: 100%;
  height: 100%;
  display:flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 2px solid #00B4CC;
  border-radius: 30px;
}

.currency__code {
  font-size: 26px;
  font-style: bold;
  font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}

.currency__text {
  font-size: 20px;
  font: bolder;

}

.currency__country {
  font-size: 20px;
  font-style: italic;
  font: bolder;
}
</style>