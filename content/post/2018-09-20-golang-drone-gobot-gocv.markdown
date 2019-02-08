---
title: "Hacking Drone s frameworkem GoBot a knihovnou GoCV (OpenCV)"
date: 2018-09-20T10:46:45+02:00
tags:
  - golang
  - gobot
---

Ve volném časem se věnuju různým věcem a jedna z nich je hraní s Golangem a frameworkem pro IoT [Gobot](https://gobot.io) a [GoCV](https://gocv.io/).
<!--more-->
Gobot umí pracovat s mnoha zařízeními, já si hraju s dronem [Tello](https://store.dji.com/product/tello). Dron má 5Mpx kameru a můžete ho ovládat přes wifi. Protokol pro ovládání je celkem jednoduchý. Můžete používat ho přes UDP a to buď v ASCII nebo binárně.

Tady je jednoduchý příklad, kde drone vzlétne a po chvíli přistane.

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
$ echo -n "command" | netcat -u 192.168.10.1 8889
$ echo -n "takeoff" | netcat -u 192.168.10.1 8889
$ echo -n "land" | netcat -u 192.168.10.1 8889
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

Drone umí pracovat s videm a já to používám na práci s OpenCV knihovnou. Používam detekci zmíněnou tady [Face detection](https://gocv.io/writing-code/face-detect/) kde se použije externí CascadeClassifier - soubor s modelem, který se použije. Program pracuje s každým snímkem a klasifikátor se použije k detekci tváře. Pokud najde tvář, nakreslí zelený obdelník kolem každé jak je vidět na obrázku:

![](https://raw.githubusercontent.com/hybridgroup/gocv/master/images/face-detect.jpg)

kód kombinuje GoCV (wrapper kolem OpenCV) a GoBot.

```golang
/*
You must have ffmpeg and OpenCV installed in order to run this code. It will connect to the Tello
and then open a window using OpenCV showing the streaming video.

How to run

	go run examples/tello_opencv.go
*/

package main

import (
	"fmt"
	"image"
	"image/color"
	"io"
	"os/exec"
	"strconv"
	"time"

	"gobot.io/x/gobot"
	"gobot.io/x/gobot/platforms/dji/tello"
	"gocv.io/x/gocv"
)

const (
	frameX    = 400
	frameY    = 300
	frameSize = frameX * frameY * 3
)

func main() {
	drone := tello.NewDriver("8890")
	window := gocv.NewWindow("Tello")
	xmlFile := "haarcascade_frontalface_default.xml"
	ffmpeg := exec.Command("ffmpeg", "-hwaccel", "auto", "-hwaccel_device", "opencl", "-i", "pipe:0",
		"-pix_fmt", "bgr24", "-s", strconv.Itoa(frameX)+"x"+strconv.Itoa(frameY), "-f", "rawvideo", "pipe:1")
	ffmpegIn, _ := ffmpeg.StdinPipe()
	ffmpegOut, _ := ffmpeg.StdoutPipe()

	work := func() {
		if err := ffmpeg.Start(); err != nil {
			fmt.Println(err)
			return
		}

		drone.On(tello.ConnectedEvent, func(data interface{}) {
			fmt.Println("Connected")
			drone.StartVideo()
			drone.SetVideoEncoderRate(tello.VideoBitRateAuto)
			drone.SetExposure(0)

			gobot.Every(100*time.Millisecond, func() {
				drone.StartVideo()
			})
		})

		drone.On(tello.VideoFrameEvent, func(data interface{}) {
			pkt := data.([]byte)
			if _, err := ffmpegIn.Write(pkt); err != nil {
				fmt.Println(err)
			}
		})
	}

	robot := gobot.NewRobot("tello",
		[]gobot.Connection{},
		[]gobot.Device{drone},
		work,
	)

	// calling Start(false) lets the Start routine return immediately without an additional blocking goroutine
	robot.Start(false)

	// now handle video frames from ffmpeg stream in main thread, to be macOS/Windows friendly
	for {
		buf := make([]byte, frameSize)
		if _, err := io.ReadFull(ffmpegOut, buf); err != nil {
			fmt.Println(err)
			continue
		}
		img, _ := gocv.NewMatFromBytes(frameY, frameX, gocv.MatTypeCV8UC3, buf)
		if img.Empty() {
			continue
		}

		// detect faces
		// color for the rect when faces detected
		blue := color.RGBA{0, 0, 255, 0}
		// load classifier to recognize faces
		classifier := gocv.NewCascadeClassifier()
		defer classifier.Close()
		if !classifier.Load(xmlFile) {
			fmt.Printf("Error reading cascade file: %v\n", xmlFile)
			return
		}
		rects := classifier.DetectMultiScale(img)
		fmt.Printf("found %d faces\n", len(rects))

		// draw a rectangle around each face on the original image,
		// along with text identifying as "Human"
		for _, r := range rects {
			gocv.Rectangle(&img, r, blue, 3)

			size := gocv.GetTextSize("Human", gocv.FontHersheyPlain, 1.2, 2)
			pt := image.Pt(r.Min.X+(r.Min.X/2)-(size.X/2), r.Min.Y-2)
			gocv.PutText(&img, "Human", pt, gocv.FontHersheyPlain, 1.2, blue, 2)
		}

		window.IMShow(img)
		if window.WaitKey(1) >= 0 {
			break
		}
	}
}
```

Pokud chcete začít si hrát s dronem, můžete použít přímo nástroje v linuxu nebo mac os x jako je netcat kdy jste schopni posílat příkazy přímo dronu pomocí síťového protokolu UDP.

Když si chcete více hrát je dobré zapojit programovácí jazyk. Jednoduchý klient pro UDP napíšete v libovolném jazyce, mi jsme na Webexpu pracovali s Javascriptem a výsledky byli pěkné.

Na práci s videem zatím nikdo nerozšířil Node.js verzi, ale určitě to půjde a časem to někdo dodělá. V Gobot frameworku to funguje, tam taky čteme video a dekodujeme ho pomocí `ffmpeg` a potom zpracujeme jednotlivé snímky. Například můžeme detekovat pohyb před kamerou, rozpoznat koho vidíme a detekovat, že vidíme lidi pomocí knihovny OpenCV.

