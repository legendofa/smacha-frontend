<!--
<script>
    import mqtt from "mqtt";
    
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
    
    const topic = 'sensors'
    client.on('connect', () => {
      console.log('Connected')
      client.subscribe([topic], () => {
        console.log(`Subscribe to topic '${topic}'`)
      })
    })
    
    let temperature_values = []
    let humidity_values = []
    client.on('message', (/** @type {any} */ topic, /** @type {{ toString: () => string; }} */ payload) => {
      console.log('Received Message:', topic, payload.toString())
      const data = JSON.parse(payload.toString())
      const timestamp = data['timestamp']
      const temperature = data['values']['temperature']
      // check if timestamp is already in the array, the timestamps may not be in order
      //const index = temperature_values.findIndex((element) => element.x === timestamp)
    
      temperature_values.push({ x: timestamp, y: temperature })
      const humidity = data['values']['humidity']
      humidity_values.push({ x: timestamp, y: humidity })
    })
    
    </script>
-->