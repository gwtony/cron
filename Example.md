[EXAMPLE]

package main

import (
	"fmt"
	"time"
	cron "github.com/gwtony/dcron/handler"
)

type JMeta struct {
	owner string
	age   int
}

func job(m *cron.JobMeta) {
	id := m.Id
	//var jm *JMeta
	if jm, ok := m.Data.(*JMeta); ok {
		fmt.Printf("Id: %s, Every %d second, owner: %s\n", id, jm.age, jm.owner)
	}
}

func main() {
	m1 := &JMeta{owner: "alice",age: 3}
	m2 := &JMeta{owner: "bob", age: 5}
	m3 := &JMeta{owner: "cindy", age: 10}
	meta1 := &cron.JobMeta{Id: "1", Data: m1}
	meta2 := &cron.JobMeta{Id: "2", Data: m2}
	meta3 := &cron.JobMeta{Id: "3", Data: m3}
	c := cron.New()
	c.AddFunc("*/3 * * * * *", meta1, job)
	c.AddFunc("*/5 * * * * *", meta2, job)
	c.AddFunc("*/10 * * * * *", meta3, job)
	//c.AddFunc("*/13 * * * * *", meta3, func() { fmt.Println("Every 13 second") })
	//c.AddFunc("@hourly", meta, func() { fmt.Println("Every hour") })
	//c.AddFunc("@every 1h30m", meta, func() { fmt.Println("Every hour thirty") })
	c.Start()
	time.Sleep(time.Second * 1000)
	c.Stop()  // Stop the scheduler (does not stop any jobs already running).

}
