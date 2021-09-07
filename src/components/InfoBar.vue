<template>
  <div class="bars">
    <div class="bar" :style="{flexGrow: props.value1, backgroundColor:'Gold'}" />
    <div class="bar" :style="{flexGrow: props.value2, backgroundColor:'SteelBlue'}" />
    <div class="bar" :style="{flexGrow: props.value3, backgroundColor:'LightGreen'}" />
  </div>

  <div class="legend">
      <div class="legend-item">
        <div class="legend-item__box" style="background:Gold" />
        <div class="legend-item__label">kujundamine</div>
      </div>
      <div class="legend-item">
        <div class="legend-item__box" style="background:SteelBlue" />
        <div class="legend-item__label">kodeerimine</div>
      </div>
      <div class="legend-item">
        <div class="legend-item__box" style="background:LightGreen" />
        <div class="legend-item__label">asjaajamine</div>
      </div>
    </div>
</template>

<script>

export default {
  props : {
    value1: {
      type: Number,
      default: 0
    },
    value2: {
      type: Number,
      default: 0
    },
    value3: {
      type: Number,
      default: 0
    },
  },
  setup(props) {
    return {props}
  },
};
</script>

<style>
  .bars{
    align-self: flex-end;
    display: flex;
    margin-top: 5vh;
    width:100%;
    border-radius: 1rem;
    overflow: hidden;
    box-shadow: 0 0.25rem 0.25rem 0 hsla(0, 0%, 0%, 0.15);
  }
  .bar{
    height: 1em;
    transition: all 0.5s ease-in-out;
  }
  .legend{
    margin-top: 2vh;
    display: flex;
    justify-content: space-between;
  }
  .legend-item{
    display: flex;
    align-items: center;
    gap: 0.25rem;
  }
  .legend-item__box{
    width:1rem;
    height:1rem;
    border-radius: 50%;
  }
  .legend-item__label{
    font-size: 0.75rem;
  }
</style>