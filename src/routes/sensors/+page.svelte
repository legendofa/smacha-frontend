<script lang="ts">
    import TopBar from "$lib/TopBar.svelte";
    import '@carbon/charts-svelte/styles.css'
    import { BarChartSimple, LineChart, GaugeChart, ScaleTypes, MeterChart, DonutChart } from '@carbon/charts-svelte'
    import Card from "@smui/card";
    import { onMount } from 'svelte';
    import { tr } from "date-fns/locale";

    const endpoint = "http://localhost:5000";
    let humidity_data: { group: string, date: string, value: number }[] = [];
    let latest_humidity: { group: string, value: number }[] = [{group: 'Dataset 1', value: 0}];
    let temperature_data: { group: string, date: string, value: number }[] = [];
    let latest_temperature: { group: string, value: number }[] = [{group: 'Dataset 1', value: 0}];
    let wall_plug_current_data: { group: string, date: string, value: number }[] = [];
    let latest_wall_plug_current: { group: string, value: number }[] = [{group: 'Dataset 1', value: 0}];
    let solar_panel_current_data: { group: string, date: string, value: number }[] = [];
    let latest_solar_panel_current: { group: string, value: number }[] = [{group: 'Dataset 1', value: 0}];

    // TODO: check if the timestamps are in correct order
    async function getData() {
        // Fetch humidity data
        const res_hum = await fetch(endpoint + "/get_humidity_data");
        let response_hum = await res_hum.json();
        response_hum = JSON.parse(response_hum);

        response_hum = response_hum.map((entry: {_id: any, humidity: number, timestamp: string}) => {
            return {
                group: 'Dataset 1',
                date: entry.timestamp,
                value: entry.humidity
            };
        });

        humidity_data = [
            ...response_hum
        ];
        if (response_hum.length > 0) {
            latest_humidity = [{group: 'Dataset 1', value: response_hum[response_hum.length - 1].value}]; //TODO: Last value is not always the latest
        }

        // Fetch temperature data
        const res_temp = await fetch(endpoint + "/get_temperature_data");
        let response_temp = await res_temp.json();
        response_temp = JSON.parse(response_temp);

        response_temp = response_temp.map((entry: {_id: any, temperature: number, timestamp: string}) => {
            return {
                group: 'Dataset 1',
                date: entry.timestamp,
                value: entry.temperature
            };
        });

        temperature_data = [
            ...response_temp
        ];
        if (response_temp.length > 0) {
            latest_temperature = [{group: 'Dataset 1', value: response_temp[response_temp.length - 1].value}]; //TODO: Last value is not always the latest
        }

        // Fetch wall plug current data
        const res_wall_plug_current = await fetch(endpoint + "/get_wall_plug_data");
        let response_wall_plug_current = await res_wall_plug_current.json();
        response_wall_plug_current = JSON.parse(response_wall_plug_current);

        response_wall_plug_current = response_wall_plug_current.map((entry: {_id: any, current: number, timestamp: string}) => {
            return {
                group: 'Dataset 1',
                date: entry.timestamp,
                value: entry.current
            };
        });

        wall_plug_current_data = [
            ...response_wall_plug_current
        ];
        if (response_wall_plug_current.length > 0) {
            latest_wall_plug_current = [{group: 'Dataset 1', value: response_wall_plug_current[response_wall_plug_current.length - 1].value}]; //TODO: Last value is not always the latest
        }

        // Fetch solar panel current data
        const res_solar_panel_current = await fetch(endpoint + "/get_solar_panel_data");
        let response_solar_panel_current = await res_solar_panel_current.json();
        response_solar_panel_current = JSON.parse(response_solar_panel_current);

        response_solar_panel_current = response_solar_panel_current.map((entry: {_id: any, current: number, timestamp: string}) => {
            return {
                group: 'Dataset 1',
                date: entry.timestamp,
                value: entry.current
            };
        });

        solar_panel_current_data = [
            ...response_solar_panel_current
        ];
        if (response_solar_panel_current.length > 0) {
            latest_solar_panel_current = [{group: 'Dataset 1', value: response_solar_panel_current[response_solar_panel_current.length - 1].value}]; //TODO: Last value is not always the latest
        }
    }

    onMount(async () => {
        while (true) {
            await getData();
            await new Promise(r => setTimeout(r, 5 * 1000)); // 5 seconds
        }
    });
</script>

<TopBar></TopBar>

<!--TODO: Add option to enable and disable zoom bar-->
<div class="card-container">
    <div class="card">
        <Card padded>
            <MeterChart
                data={latest_humidity}
                options={
                    {
                        title: 'Current humidity',
                        meter: {
                            peak: 80,
                            showLabels: false,
                        },
                        height: '100px',
                    }
                }>
            </MeterChart>
        <LineChart
        data={humidity_data}
        options={{
                title: 'Humidity over time',
                axes: {
                    left: {
                        mapsTo: 'value'
                    },
                    bottom: {
                        scaleType: ScaleTypes.TIME,
                        mapsTo: 'date'
                    }
                },
                legend: {
                    clickable: false
                },
                height: '400px',
                zoomBar: {
                    updateRangeAxis: true,
                    top: {
                        enabled: true
                    }
                }
        }}>
        </LineChart>
    </Card>
</div>
<div class="card">
    <Card padded>
        <MeterChart
            data={latest_temperature}
            options={
                {
                    title: 'Current temperature',
                    meter: {
                        peak: 40,
                        showLabels: false,
                    },
                    height: '100px',
                }
            }>
        </MeterChart>
    <LineChart
    data={temperature_data}
    options={{
            title: 'Temperature over time',
            axes: {
                left: {
                    mapsTo: 'value'
                },
                bottom: {
                    scaleType: ScaleTypes.TIME,
                    mapsTo: 'date'
                }
            },
            legend: {
                clickable: false
            },
            height: '400px',
            zoomBar: {
                updateRangeAxis: true,
                top: {
                    enabled: true
                }
            }
    }}>
    </LineChart>
</Card>
</div>
<div class="card">
    <Card padded>
        <MeterChart
            data={latest_wall_plug_current}
            options={
                {
                    title: 'Current wall plug current',
                    meter: {
                        peak: 40,
                        showLabels: false,
                    },
                    height: '100px',
                }
            }>
        </MeterChart>
    <LineChart
    data={wall_plug_current_data}
    options={{
            title: 'Current over time from wall plug',
            axes: {
                left: {
                    mapsTo: 'value'
                },
                bottom: {
                    scaleType: ScaleTypes.TIME,
                    mapsTo: 'date'
                }
            },
            legend: {
                clickable: false
            },
            height: '400px',
            zoomBar: {
                updateRangeAxis: true,
                top: {
                    enabled: true
                }
            }
    }}>
    </LineChart>
</Card>
</div>
<div class="card">
    <Card padded>
        <MeterChart
            data={latest_solar_panel_current}
            options={
                {
                    title: 'Current solar panel current',
                    meter: {
                        peak: 40,
                        showLabels: false,
                    },
                    height: '100px',
                }
            }>
        </MeterChart>
    <LineChart
    data={solar_panel_current_data}
    options={{
            title: 'Current over time from solar panel',
            axes: {
                left: {
                    mapsTo: 'value'
                },
                bottom: {
                    scaleType: ScaleTypes.TIME,
                    mapsTo: 'date'
                }
            },
            legend: {
                clickable: false
            },
            height: '400px',
            zoomBar: {
                updateRangeAxis: true,
                top: {
                    enabled: true
                }
            }
    }}>
    </LineChart>
</Card>
</div>
<!--
<div class="card">
    <Card padded>
        <MeterChart
        data={
            [
            {
                group: 'Dataset 1',
                value: 56
            }
            ]
        }
        options={
            {
                title: 'Current brightness',
                meter: {
                    peak: 80,
                    showLabels: false,
                },
                height: '100px',
            }
        }>
    </MeterChart>
    <LineChart
    data={[
        {
            group: 'Dataset 1',
            date: '2020-12-11T04:00:00.000Z',
            value: 10
        },
        {
            group: 'Dataset 1',
            date: '2020-12-11T04:30:00.000Z',
            value: 10
        },
        {
            group: 'Dataset 1',
            date: '2020-12-11T05:00:00.000Z',
            value: 2
        },
        {
            group: 'Dataset 1',
            date: '2020-12-11T05:30:00.000Z',
            value: 7
        },
        {
            group: 'Dataset 1',
            date: '2020-12-11T06:00:00.000Z',
            value: 15
        },
        {
            group: 'Dataset 1',
            date: '2020-12-11T06:30:00.000Z',
            value: 10
        }
    ]}
    options={{
        title: 'Brightness over time',
        axes: {
            left: {
                mapsTo: 'value'
            },
            bottom: {
                scaleType: ScaleTypes.TIME,
                mapsTo: 'date'
            }
        },
        legend: {
            clickable: false
        },
        height: '400px',
        zoomBar: {
            top: {
                enabled: true
            }
        }
    }}>
    </LineChart>
</Card>
</div>
<div class="card">
    <Card padded>
        <MeterChart
        data={
            [
            {
                group: 'Dataset 1',
                value: 56
            }
            ]
        }
        options={
            {
                title: 'Current car battery charge (Wh)',
                meter: {
                    peak: 80,
                    showLabels: false,
                },
                height: '100px',
            }
        }>
    </MeterChart>
    <LineChart
    data={[
        {
            group: 'Dataset 1',
            date: '2020-12-11T04:00:00.000Z',
            value: 10
        },
        {
            group: 'Dataset 1',
            date: '2020-12-11T04:30:00.000Z',
            value: 10
        },
        {
            group: 'Dataset 1',
            date: '2020-12-11T05:00:00.000Z',
            value: 2
        },
        {
            group: 'Dataset 1',
            date: '2020-12-11T05:30:00.000Z',
            value: 7
        },
        {
            group: 'Dataset 1',
            date: '2020-12-11T06:00:00.000Z',
            value: 15
        },
        {
            group: 'Dataset 1',
            date: '2020-12-11T06:30:00.000Z',
            value: 10
        }
        ]}
        options={{
            title: 'Car battery charge over time (Wh)',
            axes: {
                left: {
                    mapsTo: 'value'
                },
                bottom: {
                    scaleType: ScaleTypes.TIME,
                    mapsTo: 'date'
                },
            },
            legend: {
                clickable: false,
            },
            height: '400px',
            zoomBar: {
                top: {
                    enabled: true
                }
            },
            color: {
                gradient: {
                    colors: ["blue"]
                }
            }
        }}>
    </LineChart>
</Card>
</div>
<div class="card">
    <Card padded>
        <GaugeChart
        data={[
            {
                group: 'value',
                value: 42
            },
            ]}
            options={{
                title: 'Current car battery charge (%)',
                resizable: true,
                height: '250px',
                gauge: {
                    status: 'warning',
                    type: 'full'
                }
            }}>
        </GaugeChart>
    </Card>
</div>
<div class="card">
    <Card padded>
        <DonutChart
        data={[
            {
                group: 'Net (W)',
                value: 2.0
            },
            {
                group: 'Solar (W)',
                value: 6.5
            }]
        }
        options={{
            title: 'Current charging energy mix',
            resizable: true,
            donut: {
                center: {
                    label: 'Watt(s)'
                }
            },
            height: '400px'
        }}>
    </DonutChart>
</Card>
</div>
<div class="card">
    <Card padded>
        <DonutChart
        data={[
            {
                group: 'Net (Wh)',
                value: 1000
            },
            {
                group: 'Solar (Wh)',
                value: 5000
            }]
        }
        options={{
            title: 'Overall charging energy mix',
            resizable: true,
            donut: {
                center: {
                    label: 'Watthour(s)'
                }
            },
            height: '400px'
        }}>
    </DonutChart>
</Card>
</div>
-->
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