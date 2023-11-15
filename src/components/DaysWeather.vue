<template>
    <div class="days-tab text-center">
        <div class="container p-0">
            <div class="row">
                <div v-for="day in forecast" :key="day.date" class="col">
                    <div class="py-3"><img :src="day.iconUrl"/></div>
                    <div class="py-3">{{ getDayName(day.date) }}</div>
                    <div class="py-3">{{ day.temperature }}&deg;C</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import moment from 'moment';

    export default {
        name: 'DaysWeather',
        props: {
            cityname: String
        },
        data () {
            return {
                forecast: [],
                loading: true,
                iconUrl: true
            };
        },
        mounted() {
            this.fetchWeatherData();
        },
        methods: {
            async fetchWeatherData () {
                const apiKey = '716e2ccdcf5d9ed0b976a7a9480465ec';
                const city = this.cityname;
                const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apiKey}`;
                try {
                    const api = await fetch (apiUrl)
                    const responseData = await api.json();

                    if (responseData.cod === "404" || responseData.cod === "400") {
                        throw new Error(responseData.message);
                    } 

                    const forecastData =  responseData.list;
                    const filterData = forecastData.map( item => {
                        return {
                            date: moment(item.dt_txt.split(' ')[0]),
                            temperature: Math.round(item.main.temp),
                            description: item.weather[0].description,
                            iconUrl: `https://api.openweathermap.org/img/w/${item.weather[0].icon}.png`,
                        }
                    }).reduce((acc, item) => {
                        if(!acc.some(day => day.date.isSame(item.date, 'day'))){
                            acc.push(item);
                        }
                        return acc;
                    }, []).slice(1, 5);

                    this.forecast = filterData;
                    this.loading = false;
                } catch (error) {
                    console.log(error);
                }
            },
            getDayName(date) {
                return date.format('ddd');
            }
        }
    }
</script>

<style>

.days-tab {
    width: 100%;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    border-radius: 20px;
    margin: 80px 0 80px 0;
}

.loading {
    color: #fff;
}

.container {
    color: white;
}

.row {
    padding: 50px;
}

span {
    display: block;
    margin-bottom: 5px;
    font: 100% sans-serif;
    height: 35px;
}

.col {
    background: #263d5c;
    color: #fff;
    border-radius: 10px;
    margin: 0.5rem;
    font-weight: 600;
}

.col:hover {
    transform: scale(1.2);
    transition: transform 0.1s ease;
}

.col-temp {
    display: inline-block;
    background-color: #222831;
    color: #fff;
    transition: background-color 0.5s;
    border-radius: 10px;
}

</style>