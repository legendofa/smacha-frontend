<script>
    import TopBar from "$lib/TopBar.svelte";
    import Card from '@smui/card';
    import FormField from '@smui/form-field';
    import Radio from '@smui/radio';
    import AutomaticControl from "$lib/AutomaticControl.svelte";
    import OverwriteControl from "$lib/OverwriteControl.svelte";
    
    
    let options = ['Automatic', 'Overwrite'];
    let selected = 'Automatic';
</script>

<TopBar></TopBar>

<div class="card-container">
    <Card padded>   
        {#each options as option}
        <FormField>
            <Radio
            bind:group={selected}
            value={option}
            />
            <span slot="label">
                {option}
            </span>
        </FormField>
        {/each}
        {#if selected == 'Automatic'}
            <AutomaticControl></AutomaticControl>
        {/if}
        {#if selected == 'Overwrite'}
            <OverwriteControl></OverwriteControl>
        {/if}
    </Card>
</div>

<style>
    .card-container {
        margin-top: 70px;
        width: min-content;
        margin-left: auto;
        margin-right: auto;
    }
</style>