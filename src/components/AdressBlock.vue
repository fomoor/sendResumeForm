<template>
    <div class="map">
        <div id="map" :class="currentCity"></div>                
        <div class="contacts">
            <p class="contacts-header">Контакты:</p>
            <p><a class="number" href="tel:+74959297078">+7 495 929-70-78</a></p>
            <a class="mailto" href="mailto:welcome@ret-team.su">welcome@ret-team.su</a>
            <p class="hours">Мы работаем в будни с 9:00 до 18:00. </p>
        </div>
        
    </div>
</template>

<script>
    export default {
        name: 'AdressBlock',
        props: {
            currentCity: {
                type: String
            }
        },
        methods: {
            asdasdasd() {
                console.log(1231231);
            }
        },
        watch: {
            currentCity() {
                update(this.currentCity);
            }
        }
    }

    const nnov = [[56.297127, 43.946689], "Комсомольская площадь, 2"];
    const msc = [[55.753394, 37.627272], "Улица Ильинка, 4"];
    const kzn = [[55.785387, 49.110905], "Улица Галиаскара Камала, 5А"];
    const ksdr = [[45.045434, 38.975719], "Рашпилевская улица, 179/1"];

    function coordinates(city) {
        return city === "nnov" ? nnov[0] : city === "msc" ? msc[0]  : city === "kzn" ? kzn[0]  : ksdr[0];
    }

    function streets(city) {
        return city === "nnov" ? nnov[1] : city === "msc" ? msc[1]  : city === "kzn" ? kzn[1]  : ksdr[1];
    }
    
    var myMap = null;
        
    ymaps.ready(() => {

        let city = document.querySelector("#map").classList.value;

        myMap = new ymaps.Map('map', {
            center: coordinates(city),
            zoom: 16,
            controls: []
        });

        document.querySelector('.ymaps-2-1-79-copyrights-pane').remove();

        var MyIconContentLayout = ymaps.templateLayoutFactory.createClass(
            '<div style="font-weight: 500; font-size: 15px; background: #fff; margin-top: 60px; margin-left: -100px; width: 247px; padding: 8px 0; box-shadow: 0px 0px 15px rgba(184, 178, 178, 0.5); border-radius: 5px;">$[properties.iconContent]</div>'
        );

        const baloonSrc = {
            iconLayout: 'default#imageWithContent',
            iconImageHref: '../images/mapIcon.gif',
            iconImageSize: [50, 50],
            iconImageOffset: [-25, -30],
            iconContentLayout: MyIconContentLayout
        };

        var nnovPlacemark = new ymaps.Placemark(coordinates('nnov'), {
            iconContent: streets('nnov')
        }, baloonSrc);
        var mscPlacemark = new ymaps.Placemark(coordinates('msc'), {
            iconContent: streets('msc')
        }, baloonSrc);
        var kznPlacemark = new ymaps.Placemark(coordinates('kzn'), {
            iconContent: streets('kzn')
        }, baloonSrc);
        var ksdrPlacemark = new ymaps.Placemark(coordinates('ksdr'), {
            iconContent: streets('ksdr')
        }, baloonSrc);

        myMap.geoObjects
            .add(nnovPlacemark)
            .add(mscPlacemark)
            .add(kznPlacemark)
            .add(ksdrPlacemark);

        update(city);

    });

    function update(city) {
        if (!myMap) return;
        myMap.setCenter(coordinates(city), 16);
    };

</script>