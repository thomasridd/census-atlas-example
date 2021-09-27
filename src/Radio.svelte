<script>
    export let props;
    export let selected;
</script>

<style>
    .radios__item {
        min-width: 0 !important;
    }
</style>

<div class="radios__item">
    <span class="radio">
        <input type="radio" id="{props.name}-radio" bind:group={selected} class="radio__input  js-radio" value={props}>
        <label class="radio__label  " for="{props.name}-radio" id="{props.name}-label">{props.name}</label>
    </span>
</div>