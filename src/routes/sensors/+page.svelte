<script>
    import TopBar from "$lib/TopBar.svelte";
    import '@carbon/charts-svelte/styles.css'
    import { BarChartSimple, LineChart, ScaleTypes } from '@carbon/charts-svelte'
    import Card from "@smui/card";
</script>

<TopBar></TopBar>

<div class="card-container">
    <div class="card">
        <Card padded>
            <BarChartSimple
            data={[
                { group: 'Qty', value: 65000 },
                { group: 'More', value: 29123 },
                { group: 'Sold', value: 35213 },
                { group: 'Restocking', value: 51213 },
                { group: 'Misc', value: 16932 }
                ]}
                options={{
                    title: 'Simple bar (discrete)',
                    height: '400px',
                    axes: {
                        left: { mapsTo: 'value' },
                        bottom: { mapsTo: 'group', scaleType: ScaleTypes.LABELS }
                    }
                }}>
            </BarChartSimple>
        </Card>
    </div>
    <div class="card">
        <Card padded>
            <LineChart
            data={[
                { group: 'Qty', value: 65000 },
                { group: 'More', value: 29123 },
                { group: 'Sold', value: 35213 },
                { group: 'Restocking', value: 51213 },
                { group: 'Misc', value: 16932 }
                ]}
                options={{
                    title: 'Simple line chart (discrete)',
                    height: '400px',
                    axes: {
                        left: { mapsTo: 'value' },
                        bottom: { mapsTo: 'group', scaleType: ScaleTypes.LABELS }
                    }
                }}>
            </LineChart>
        </Card>
    </div>
    <div class="card">
        <Card padded>
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
                    title: 'Line (time series) - Time interval 30minutes',
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
                    height: '400px'
                }}>
            </LineChart>
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
        width: 500px;
    }
</style>