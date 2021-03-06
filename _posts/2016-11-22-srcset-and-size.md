---
layout: post
title: 'Атрибуты srcset и&nbsp;size'
discription: Использование атрибутов srcset и size или "как сэкономить трафик и нервы пользователям" 
pageId: article
links:
- url: http://w3c.github.io/html/semantics-embedded-content.html
- url: https://www.sitepoint.com/how-to-build-responsive-images-with-srcset/
- url: https://css-tricks.com/responsive-images-youre-just-changing-resolutions-use-srcset/ 
---

Короткая статья об атрибуте `srcset`, который позволяет нам отдавать изображения в зависимости 
от характеристик устройств, с которых пользователи просматривают твой сайт.

Сёрфя по интернету с зашкваренным телефоном, очень раздражает медленная загрузка изображений. Помимого того,
что мой телефон поддерживает только 2G, скорее всего добрый &laquo;человек-разработчик&raquo; позаботился о том, чтобы все 
изображения были 2x-размера (которые прекрасно будут смотреться на устройствах с ретина-дисплеями).
 
К сожалению, я не длинноногая блондинка с богатым внутренним миром и iphone мне никто не подарил,	
а загружать большие картинки на свой маленький телефон&nbsp;&mdash; совсем не камильфо.

<figure class="post__img-container">
	<img src="/assets/images/pit.jpg" alt="Плачущий Питер парке" />
</figure>

От слов сразу к делу. Всё что нужно&nbsp;&mdash; это просто добавить атрибут `srcset` в наш тэг `<img>`, в котором
через запятую мы перечислим пути к нашим изображениям и значения плотности пекселей экранов (исходя из которых эти изображения будут загружаться):

{% highlight html %}
<img src="50cent.jpg" 
     srcset="50cent-medium.jpg 1.5x, 50cent-large.jpg 2x" 
     alt="50cent" 
/>
{% endhighlight %}

Также мы оставили родной атрибут `src`, который будет выступать в качестве фолбэка (в нашем случае он указывает на изображение наименьшего размера). Если вы уверены, 
что вам не нужен фолбэк, можно смело его сносить и писать так: 

{% highlight html %}
<img srcset="50cent-medium.jpg 1x, 50cent-medium.jpg 1.5x, 50cent-large.jpg 2x" 
     alt="50cent" 
/>
{% endhighlight %}

Эта техника подходит для отображения одного и того же изображения на разных устройствах. На устройствах 
с большей плотностью пикселей будет отображаться изображение с бОльшим разрешением, но ширина и высота 
изображения останутся фиксированными. 

Эм, что? 

К примеру, у нас есть изображение шириной 400px, которое будет выводиться на экранах с обычной плотностью и 
изображение шириной 800px, которое мы будем показывать на экранах с плотностью пикселей 2x. 
Важно понимать, что на экранах с повышенной плотностью пикселей наше изображение шириной 800px ужмётся в 2 раза, 
согласно дескриптору, т.е. его ширина будет 400px. Таким образом, на разных экранах с разной 
плотностью пикселей наше изображение будет иметь один и тот же размер.

Всё. Реально. На этом можно уже остановиться. Но... 

Помимо дескриптора `x` `srcset` поддерживает дескриптор `w`&nbsp;&mdash; ширина. С его помощью мы сообщаем
браузеру о ширине изображений:

{% highlight html %}
<img src="50cent.jpg" 
     srcset="50cent-medium.jpg 500w, 50cent-large.jpg 1000w" 
     alt="50cent" 
/>
{% endhighlight %}

<strong>w = px</strong>. То есть делая такую запись `50cent-medium.jpg 800w`, мы говорим пользовательскому агенту,
что это изображение шириной 800px. Указание размера изображения без атрибута `size`, штука, конечно,
бесполезная, сродни диплому о высшем образовании. Он как бы есть, но как бы пофиг. Вдобавок
`w` прекрасно переводится в `x`. К примеру, дан экран размером 768 грязных CSS пикселей, 
тогда верхний пример будет выглядеть так: 
 
{% highlight html %}
<img src="50cent.jpg" 
     srcset="50cent-medium.jpg 0.6x, 50cent-large.jpg 1.3x" 
     alt="50cent" 
/>
{% endhighlight %}
 
Расчёты осуществляем согласно каноничной формуле Сами-Знаете-Кого:

<b>цель / контекст = результат</b>,

где <i>размеры изображения = цель</i>, а <i>размер экрана = контекст</i>.

Хотелось бы отметить, что aтрибут `srcset` -  это <strong>не жёсткое</strong> указание браузеру 
(какое изображение он должен применить), а скорее мягкая, как твоя реакция в CS:GO, рекомендация. 
Если резюмировать написанное в спецификации, то там сказано, что браузер сам выберет изображение из 
имеющегося перечня, руководствуясь плотностью пикселей экрана пользователя, масштабированием и, возможно,
другими факторами, такими как характеристики сетевого соединения.  

![Мем Okay](/assets/images/okay.jpg)

## Size

Парам-пам-пам... пришло время `size`: 

{% highlight html %}
<img src="50cent.jpg" 
     srcset="50cent-medium.jpg 500w, 50cent-large.jpg 1000w" 
     sizes="50vw" 
     alt="50cent"
/>
{% endhighlight %}

В атрибуте `size` мы указываем, какую площадь экрана будет занимать изображение (надеюсь, 
ты знаком с единицами измерения `vw` и им подобным, потому что это действительно клёвая вещь). В нашем 
случае мы говорим браузеру: "э, братуха, а запили нам, по-пацански, картиночку на пол экрана". Ноу проблем.
Браузер сам решит, какое изображение больше подходит под заданные критерии и загрузит его. Эх, везде бы так. 

Конечно, вместо `w` можно использовать `x`: 

{% highlight html %}
<img src="50cent.jpg" 
     srcset="50cent-medium.jpg 0.6x, 50cent-large.jpg 1.3x" 
     sizes="50vw"
     alt="50cent" 
/>
{% endhighlight %}

А теперь вкусняшка: `size`  поддерживает, своего рода, медиа-запросы: 

{% highlight html %}
<img src="50cent.jpg" 
     srcset="50cent-medium.jpg 500w, 50cent-large.jpg 1000w" 
     sizes="(max-width: 768px) 100vw, 50vw"
     alt="50cent"
/>
{% endhighlight %}

Как с медиазапросами: если максимальная ширина экрана равна или меньше 768px, будь добр, выводи 
изображение на всю ширину. Для всего остального есть 50vw. 

Можно совмещать несколько медиа-запросов:

{% highlight html %}
<img src="50cent.jpg" 
     srcset="50cent-medium.jpg 500w, 50cent-large.jpg 1000w" 
     sizes="(max-width: 768px) 100vw, (max-width: 1024px) 50vw, calc(33vw - 100px)"
     alt="50cent"
/>
{% endhighlight %}

Тут почти всё тоже самое, последнее условие будет применяться, если предшествующие вернут `false`.

А вот так работать не будет (я встречал такую запись в одной книге):

{% highlight html %}
<img src="50cent.jpg" 
     srcset="50cent-medium.jpg 500w, 50cent-large.jpg 1000w" 
     sizes="(min-width: 768px) 100vw, (min-width: 1024px) 50vw, calc(33vw - 100px)"
     alt="50cent"
/>
{% endhighlight %}

Думаю, понятно почему: второй медиа-запрос входит в область видимости первого, поэтому срабатывать
будет только первый медиа-запрос. Используем &laquo;ракообразную технику&raquo;, чтобы этого избежать: 

{% highlight html %}
<img src="50cent.jpg" 
     srcset="50cent-medium.jpg 500w, 50cent-large.jpg 1000w" 
     sizes="(min-width: 1024px) 100vw, (min-width: 768px) 50vw, calc(33vw - 100px)"
     alt="50cent"
/>
{% endhighlight %}

Конечно, `srcset` и `size` крутые вещи, но тэг `picture` даёт нам больше контроля и позволяет выбирать
изображения опираясь на их типы. Но об этом в другой раз. 

<strong>P.S.</strong> Не забываем переодически чистить <b>кэш</b> при тестировании.

Досвидули, досвидули.














