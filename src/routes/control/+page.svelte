<script>
    import TopBar from "$lib/TopBar.svelte";
    import Card from '@smui/card';
    import FormField from '@smui/form-field';
    import Switch from '@smui/switch';
    import Button from '@smui/button';
    import { Label } from '@smui/button';
    import Textfield from '@smui/textfield';
    import mqtt from "mqtt";    
    import CollapsibleCard from 'svelte-collapsible-card'
    import { onMount } from 'svelte';
    
    let mWhTrip = 0;
    let onTrip = false;
    let scheduleUse = false;
    let currentlyCharging = false;
    let chargingSpeed = "";
    let chargingType = "";
    let showPlan = false;

    const protocol = 'mqtt'
    const host = 'raspberrypi.local'
    const port = '8080'
    const clientId = `iot-frontend_${Math.random().toString(16).slice(3)}`
    
    const connectUrl = `${protocol}://${host}:${port}`
    
    const client = mqtt.connect(connectUrl, {
      clientId,
      clean: true,
      connectTimeout: 4000,
      //username: 'sciot-user',
      //password: '^7pKERzqawYhZtZh29d3',
      reconnectPeriod: 1000,
    })

    client.on('connect', () => {
        console.log('Connected')
        client.subscribe(['/iot-backend/+'], () => {
            console.log(`Subscribe to topic /iot-backend/+`)
        })
    })

    client.on('message', (topic, payload) => {
        console.log('Received Message:', topic, payload.toString())

        if (topic === '/iot-backend/charging-status') {
            currentlyCharging = payload.toString() == 'True'
            console.log('currentlyCharging:', currentlyCharging)
        } else if (topic === '/iot-backend/current-plan') {
            let data = JSON.parse(payload.toString())
            chargingSpeed = data['charge_speed']
            chargingType = data['charge_type']

            console.log('chargingSpeed:', chargingSpeed)
            console.log('chargingType:', chargingType)
        }
    })

    function handleScheduleUse() {
        console.log('publishing schedule-use:', scheduleUse)
        client.publish('frontend/schedule-use', String(scheduleUse), { qos: 2, retain: false })
    }

    async function startTrip() {
        onTrip = true;
        let tripDuration = (mWhTrip / 12000) * 3600;
        console.log('Starting Tip with mW:', mWhTrip)
        console.log('Trip Duration:', tripDuration)

        let msg = {
            "mWh": mWhTrip,
            "duration": tripDuration
        }

        client.publish('frontend/trip-start', JSON.stringify(msg), { qos: 2, retain: false })

        await new Promise(r => setTimeout(r, tripDuration * 1000));
        onTrip = false;
        client.publish('frontend/trip-end', '', { qos: 2, retain: false })
    }

    const endpoint = "http://localhost:5000";
    let average_humidity = 0;
    let average_temperature = 0;

    async function getData() {
        const response_avg_hum = await fetch(endpoint + "/get_average_humidity_data");
        const data_avg_hum = await response_avg_hum.json();
        //console.log(data_avg_hum);
        average_humidity = data_avg_hum

        const response_avg_temp = await fetch(endpoint + "/get_average_temperature_data");
        const data_avg_temp = await response_avg_temp.json();
        //console.log(data_avg_temp);
        average_temperature = data_avg_temp
    }

    onMount(async () => {
        while (true) {
            await getData();
            await new Promise(r => setTimeout(r, 5 * 1000)); // 5 seconds
        }
    });
</script>

<TopBar></TopBar>

<div class="card-container">
    {#if !onTrip}
        <div class="card">
        <Card padded>
            <FormField>
                <Switch bind:checked={scheduleUse} on:SMUISwitch:change={handleScheduleUse}/>
                <span slot="label">
                    Schedule use!
                </span>
            </FormField>
        </Card>
        </div>
        <div class="card">
        <Card padded>
            <Textfield type="number" variant="outlined" bind:value={mWhTrip} label="mWh"></Textfield>
            <Button on:click={startTrip}>
                <Label>Start Trip</Label>
            </Button>
        </Card>
        </div>
        <div class="card">
        <Card padded>
            <p>Humidity: {average_humidity < 50 ? "low" : "high"}</p>
            <p>Temperature: {average_temperature < 25 ? "low" : "high"}</p>
        </Card>
        </div>
        <div class="card">
            <FormField>
                <Switch bind:checked={showPlan}/>
                <span slot="label">
                    Show current plan
                </span>
            </FormField>

            {#if showPlan}
            <Card padded>
                    {#if currentlyCharging}
                        <p>Charging speed: {chargingSpeed}</p>
                        <p>Charging Type: {chargingType}</p>
                    {:else}
                        <p>Not Charging</p>
                    {/if}
            </Card>
            {/if}
        </div>
        <!--<Card padded>
            {#if currentlyCharging}
                <Button on:click={startTrip}>
                    <Label>Stop Charging</Label>
                </Button>
            {:else}
                <Button on:click={startTrip}>
                    <Label>Start Charging</Label>
                </Button>
            {/if}
        </Card>-->
    {:else}
        <div class="card">
        <Card padded>
            <h1>On Trip</h1>
        </Card>
        </div>
    {/if}    
</div>

<style>
    .card-container {
        display: flex;
        flex-wrap: wrap;
        margin-top: 70px;
        justify-content: center;
    }
    .card {
        padding: 20px;
        min-width: 500px;
    }
</style>