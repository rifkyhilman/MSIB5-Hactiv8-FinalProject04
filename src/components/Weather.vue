<template>
    <div v-if="notFound" class="not-found">  
        <img src="https://cdn-icons-png.flaticon.com/512/755/755014.png">
        <p>Oops! Invalid location :/</p>
    </div>
    <div v-else class="container p-0">
        <div class="card-1 w-100" v-bind:style= "{ 'background-image': 'linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3)), url('+ backgroundImg +')'}" >
            <div class="content text-light">
                <h2 class="place">{{ name }}, <small>{{ country }}</small></h2>
                <div class="temp">
                    <h1 class="weather-temp">{{ temperature }}&deg;</h1>
                    <p class="text-light">{{ description }} </p>
                    <img :src="iconUrl"/>
                </div>
                <p class="text-light date mb-0">{{ date }}</p>
                <small>{{ time }}</small>
            </div>
        </div>
        <div class="card-2 w-100">
            <table>
                <tbody>
                    <tr>
                        <th>Humidity</th>
                        <td>{{ humidity }}</td>
                    </tr>
                    <tr>
                        <th>Sunrise</th>
                        <td>{{ sunrise }}</td>
                    </tr>
            
                    <tr>
                        <th>wind</th>
                        <td>{{ wind }}</td>
                    </tr>
                    <tr>
                        <th>Sunset</th>
                        <td>{{ sunset }}</td>
                    </tr>
                </tbody>
            </table>
            <DaysWeather :cityname="cityname"/>
        </div>
    </div>
</template>


<script>
import DaysWeather from './DaysWeather.vue';

export default {
    name: "myWaether",
    components: {
        DaysWeather,
    },
    props: {
        city: String,
    },
    data() {
        return {
            cityname: this.city,
            temperature: null,
            description: null,
            iconUrl: null,
            date: null,
            time: null,
            name: null,
            wind: null,
            country: null,
            humidity: null,
            sunrise: null,
            sunset: null,
            notFound: false,
            backgroundImg: "",
            monthName: ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"]
        }
    },
    async created() {
        try {
            // get data
            const apiKey = '716e2ccdcf5d9ed0b976a7a9480465ec';
            const api = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${apiKey}`)
            const  weatherData = await api.json();

            if (weatherData.cod === "404" || weatherData.cod === "400") {
                this.notFound = true;
                throw new Error(weatherData.message);
            } 

            this.temperature = Math.round(weatherData.main.temp);
            this.description = weatherData.weather[0].description;
            this.name = weatherData.name;
            this.iconUrl = `https://api.openweathermap.org/img/w/${weatherData.weather[0].icon}.png`;
            this.country = weatherData.sys.country;
            
            this.wind = weatherData.wind.speed;
            this.sunrise = weatherData.sys.sunrise;
            this.sunset = weatherData.sys.sunset;
            this.humidity = weatherData.main.humidity;

            switch (weatherData.weather[0].main) {
                case 'Clear':
                    this.backgroundImg = "https://images.unsplash.com/photo-1529127110442-3f6ce0a9b287?q=80&w=1470&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D";
                    break;

                case 'Rain':
                    this.backgroundImg = "https://images.unsplash.com/photo-1534088568595-a066f410bcda?q=80&w=1351&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D";
                    break;

                case 'Thunderstorm':
                    this.backgroundImg = "https://images.unsplash.com/photo-1431440869543-efaf3388c585?q=80&w=1470&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D";
                    break;

                case 'Snow':
                    this.backgroundImg = "https://images.unsplash.com/photo-1542601098-8fc114e148e2?q=80&w=1470&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D";
                    break;

                case 'Clouds':
                    this.backgroundImg = "https://images.unsplash.com/photo-1597200381847-30ec200eeb9a?q=80&w=1374&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D";
                    break;

                case 'Haze':
                    this.backgroundImg = "https://images.unsplash.com/36/STzPBJUsSza3mzUxiplj_DSC09775.JPG?q=80&w=1461&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D";
                    break;

                default:
                    this.backgroundImg = "";
            }

        } catch (error) {
            console.log(error);
        }

        const d = new Date();

        this.date = d.getDate() + "/" + this.monthName[d.getMonth()] + "/" + d.getFullYear();         
        this.time = d.getHours() + ":" + d.getMinutes() + ":" + d.getSeconds();     
    }
 }

</script>

<style>

body {
    background-color: #343d4b;
}

.container {
    margin-bottom: 100px;
}

.weather-temp {
    margin: 0;
    font-weight: 700;
    font-size: 4rem;
}

h2.mb-1.day {
    font-size: 3rem;
    font-weight: 400;
}

.card-1 {
    border-radius: 20px;
    color: #fff;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    height: 80vh;
    text-align: center;
}

.temp {
    margin: 45px 0 45px 0;
}

.content {
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
}


.card-2 {
    background-color: #212730;
    border-radius: 20px;
    margin-top: 50px;
    padding: 50px 70px 50px 70px;
}

.card-details {
    margin-left: 19px;
}

.h1_left {
    position: absolute;
    bottom: 25px;
    left: 16px;
    font-size: 3vw;
    line-height: 1.2;
}

.h3_left {
    position: absolute;
    left: 16px;
    font-size: 2vw;
    line-height: 0.5;
}

.h3_left small{
    font-size: 1rem;
}

table {
    width: 100%;
    margin: 40px 0 80px 0;
}

th,
td {
    font-size: 18px;
    color: #fff;
}

td {
    text-align: right;
}


.not-found{
    width: 100%;
    text-align: center;
    margin: 100px 0 100px 0;
}

.not-found img{
    width: 250px;
    height: 250px;
}

.not-found p{
    color: white;
    font-size: 22px;
    font-weight: 500;
    margin-top: 12px;
}


</style>