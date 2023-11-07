<template>
    <div class="days-tab text-center">
        <div class="loading">Loading...</div>
        <ul class="p-0">
            <li class="li-active">
                <div class="py-3">icon</div>
                <div class="py-3">day</div>
                <div class="py-3">12oc</div>
            </li>
            <li class="li-active">
                <div class="py-3">icon</div>
                <div class="py-3">day</div>
                <div class="py-3">12oc</div>
            </li>
            <li class="li-active">
                <div class="py-3">icon</div>
                <div class="py-3">day</div>
                <div class="py-3">12oc</div>
            </li>
            <li class="li-active">
                <div class="py-3">icon</div>
                <div class="py-3">day</div>
                <div class="py-3">12oc</div>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        name: 'DaysWeather',
    }
</script>

<style>

.days-tab {
    width: 90%;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    border-radius: 20px;
    margin: auto;
}

.loading {
    color: #fff;
}

ul {
    margin: 0;
}

li {
    display: inline-block;
    list-style: none;
    height: 100%;
    width: 21%;
    max-width: 21%;
    font-size: 1vw;
    line-height: 1.2;
}

span {
    display: block;
    margin-bottom: 5px;
    font: 100% sans-serif;
    height: 35px;
}

.li-active {
    background: #263d5c;
    color: #fff;
    border-radius: 10px;
    margin: 0.5rem;
    font-weight: 600;
}

.li-active:hover {
    transform: scale(1.2);
    transition: transform 0.1s ease;
}

.li-active-temp {
    display: inline-block;
    background-color: #222831;
    color: #fff;
    transition: background-color 0.5s;
    border-radius: 10px;
}

</style>