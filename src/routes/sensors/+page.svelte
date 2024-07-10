<script>
    import TopBar from "$lib/TopBar.svelte";
    import '@carbon/charts-svelte/styles.css'
    import { BarChartSimple, LineChart, GaugeChart, ScaleTypes, MeterChart, DonutChart } from '@carbon/charts-svelte'
    import Card from "@smui/card";
</script>

<TopBar></TopBar>

<div class="card-container">
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