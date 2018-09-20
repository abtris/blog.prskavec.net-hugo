---
title: "Hacking Drone with GoBot and GoCV"
date: 2018-09-20T10:46:45+02:00
---

Ve volném časem se věnuju různým věcem a jedna z nich je hraní s Golangem a frameworkem pro IoT [Gobot](https://gobot.io) a [GoCV](https://gocv.io/).

Gobot umí pracovat s mnoha zařízeními, já si hraju s dronem [Tello](https://store.dji.com/product/tello). Dron má 5Mpx kameru a můžete ho ovládat přes wifi. Protokol pro ovládání je celkem jednoduchý. Můžete používat ho přes UDP a to buď v ASCII nebo binárně.

Tady je jednoduchý příklad

```golang
package main

import (
	"log"
	"net"
)

func main() {
	conn, err := net.ListenPacket("udp", ":0")
	if err != nil {
		log.Fatal(err)
	}
	defer conn.Close()

	dst, err := net.ResolveUDPAddr("udp", "192.168.10.1:8889")
	if err != nil {
		log.Fatal(err)
	}

	_, err = conn.WriteTo([]byte("command"), dst)
	if err != nil {
		log.Fatal(err)
	}

	_, err = conn.WriteTo([]byte("takeoff"), dst)
	if err != nil {
		log.Fatal(err)
	}

	_, err = conn.WriteTo([]byte("land"), dst)
	if err != nil {
		log.Fatal(err)
	}

}
```

nebo můžete použít `netcat`

```bash
$ netcat -u 192.168.10.1 8889
command
takeoff
land
```

pokud použijeme Gobot, kód vypadá takto:

```golang
package main

import (
	"fmt"
	"time"

	"gobot.io/x/gobot"
	"gobot.io/x/gobot/platforms/dji/tello"
)

func main() {
	drone := tello.NewDriver("8888")
	var flightData *tello.FlightData
	var battery int8
	work := func() {
		drone.TakeOff()

		drone.On(tello.FlightDataEvent, func(data interface{}) {
			flightData = data.(*tello.FlightData)
			battery = flightData.BatteryPercentage
			fmt.Println("Height:", flightData.Height)
		})

		gobot.After(5*time.Second, func() {
			fmt.Println("Battery:", battery)
			drone.Land()
		})
	}

	robot := gobot.NewRobot("tello",
		[]gobot.Connection{},
		[]gobot.Device{drone},
		work,
	)

	robot.Start()
}
```

