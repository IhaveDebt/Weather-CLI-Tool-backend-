package main

import (
	"encoding/json"
	"fmt"
	"io/ioutil"
	"net/http"
	"os"
)

const apiKey = "your_api_key_here"

type WeatherResponse struct {
	Name string `json:"name"`
	Main struct {
		Temp float64 `json:"temp"`
	} `json:"main"`
	Weather []struct {
		Description string `json:"description"`
	} `json:"weather"`
}

func main() {
	if len(os.Args) < 2 {
		fmt.Println("Usage: go run main.go [city]")
		return
	}
	city := os.Args[1]
	url := fmt.Sprintf("https://api.openweathermap.org/data/2.5/weather?q=%s&appid=%s&units=metric", city, apiKey)

	resp, _ := http.Get(url)
	body, _ := ioutil.ReadAll(resp.Body)

	var weather WeatherResponse
	json.Unmarshal(body, &weather)

	fmt.Printf("🌍 City: %s\n🌡 Temp: %.1f°C\n☁️ Weather: %s\n",
		weather.Name, weather.Main.Temp, weather.Weather[0].Description)
}
