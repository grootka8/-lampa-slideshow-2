javascript
(function () {
    "use strict";

    // Кэш для изображений
    var imageCache = {};

    // Добавление параметров в настройки
    Lampa.SettingsApi.addParam({
        component: "cardify",
        param: {
            name: "cardifyslideshowquality",
            type: "select",
            values: {
                w780: "Стандартна (w780)",
                w1280: "Висока (w1280)",
                original: "Оригінал (original)"
            },
            default: "w1280"
        },
        field: { name: "Якість зображень" }
    });

    Lampa.SettingsApi.addParam({
        component: "cardify",
        param: {
            name: "cardifyslideshowduration",
            type: "select",
            values: {
                5000: "5 секунд",
                8000: "8 секунд",
                10000: "10 секунд",
                15000: "15 секунд"
            },
            default: 8000
        },
        field: { name: "Тривалість фото (сек)" }
    });

    Follow.get(Type.de([102, 117, 108, 108]), function (e) {
        if (Type.co(e)) {
            var movie_data = e.data.movie || e.data.tv || (e.object && e.object.card);
            if (moviedata && moviedata.id) {
                loadImages(movie_data);
            }
        }
    });

    function loadImages(movie_data) {
        var itemid = moviedata.id;
        var mediatype = moviedata.media_type || 'movie';
        var currentlang = Lampa.Storage.field('tmdblang') || 'uk';

        // Проверка кэша
        if (imageCache[item_id]) {
            setupSlideshow(imageCache[item_id]);
        } else {
            var includelanguages = currentlang + ',xx,null,en';
            Lampa.Api.sources.tmdb.get(mediatype + '/' + itemid + '/images?includeimagelanguage=' + includelanguages, {}, function (imagesdata) {
                if (imagesdata && imagesdata.backdrops) {
                    var finalbackdrops = filterImages(imagesdata.backdrops, current_lang);
                    imageCache[itemid] = finalbackdrops; // Кэширование изображений
                    setupSlideshow(final_backdrops);
                }
            });
        }
    }

    function filterImages(backdrops, current_lang) {
        var lang_backdrops = [];
        var nolangbackdrops = [];

        backdrops.forEach(function (backdrop) {
            var lang = backdrop.iso6391;
            if (lang === current_lang) {
                lang_backdrops.push(backdrop);
            } else if (!lang || lang === 'xx' || lang === 'null') {
                nolangbackdrops.push(backdrop);
            }
        });

        // Собираем финальные изображения
        var finalbackdrops = langbackdrops.slice(0, 5);
        if (final_backdrops.length < 5) {
            finalbackdrops = finalbackdrops.concat(nolangbackdrops.slice(0, 5 - final_backdrops.length));
        }

        return final_backdrops;
    }

    function setupSlideshow(backdrops) {
        // Здесь реализуем логику для отображения слайд-шоу с фонами
        backdrops.forEach(function (backdrop) {
            // Показать изображение (например, добавление в DOM)
            // Можно использовать ленивую загрузку для изображений
        });
    }
})();
 
