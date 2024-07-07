
<script lang="ts">
    import Fab from '@smui/fab';
    import Textfield from '@smui/textfield';
    import Icon from '@smui/icon-button';
    import List, { Item, Meta, Label } from '@smui/list';
    import { Title } from '@smui/top-app-bar';
    
    let tripEnergyUsageWh = 0;
    let tripDateTime = '';
    
    let plannedTrips: Trip[] = [];
 
    let addTrip = () => {
        if (tripDateTime !== '' && tripEnergyUsageWh !== 0) {
            plannedTrips = [
                ...plannedTrips, 
                {
                    dateTime: tripDateTime,
                    energyUsageWh: tripEnergyUsageWh,
                }
            ];
            console.log(plannedTrips)
        }
        else {
            console.error("Trip data invalid")
        }
    };

    const deleteTrip = (index: number) => {
        plannedTrips = plannedTrips.filter((_, i) => i !== index);
    }
</script>

<div style="display: grid; grid-auto-flow: column; grid-column-gap: 10px; padding: 15px">
    <div>
    <div style="display: grid; grid-auto-flow: row; grid-row-gap: 10px">
        Tripplanner:
        <div>
            <Textfield bind:value={tripEnergyUsageWh} label="Estimated Wh Trip" type="number" />
        </div>
        <div>
            <Textfield
            bind:value={tripDateTime}
            label="Datetime Trip"
            type="datetime-local"
            />
        </div>
        <div class="margins">
            <Fab on:click={() => {addTrip()}} class="my-colored-fab" extended>
                <Icon class="material-icons">add</Icon>
                <Label>Plan trip</Label>
            </Fab>
        </div>
    </div>
</div>
    <div style="min-width: 200px">
        Planned trips:
        <List>
        {#each plannedTrips as plannedTrip, i}
        <Item>
            <Label>{plannedTrip.dateTime}</Label>
            <Meta on:click={() => deleteTrip(i)} class="material-icons">delete</Meta>
        </Item>
        {/each}
    </List>
</div>
</div>