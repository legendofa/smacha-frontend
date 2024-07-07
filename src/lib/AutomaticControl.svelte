
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

    const deleteTrip = (index) => {
        plannedTrips = plannedTrips.filter((_, i) => i !== index);
    }
</script>

<div style="display: flex">
    <div style="display: overflow">
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
    <div>
        Planned trips:
        <List>
        {#each plannedTrips as plannedTrip, i}
        <Item>
            <Label>{plannedTrip.dateTime}</Label>
            <Meta>
                <Fab on:click={() => deleteTrip(i)}>
                    <Icon class="material-icons">delete</Icon>
                </Fab>
            </Meta>
        </Item>
        {/each}
    </List>
</div>
</div>