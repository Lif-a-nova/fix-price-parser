[![image](https://avatars.mds.yandex.net/i?id=bcf6db14ce1c2655446053986d44af1b_sr-6424941-images-thumbs&n=13)](https://www.python.org/doc/) [![image](https://i2.wp.com/miro.medium.com/1*V7b3rjqIpiF1fWZd4RH0aQ.jpeg)](https://scrapy.org/)


# fix-price-parser
Парсер для получения информации о товарах по категориям из интернет-магазина "fix-price.com".
- реализовано с помощью фреймворка Scrapy
- использовано подключение через proxy (scrapy-rotating-proxies)
- на вход подается список категорий - 3 категории, с количеством товаров от 70 штук на каждую
- информация о товарах представляется в виде словарей и сохраняется в файл с расширением .json
(парссер разделен на несколько паучков с учетом доработки)

### Формат выходных данных для одного товара:
```
{
    "timestamp": int,  # Дата и время сбора товара в формате timestamp.
    "RPC": "str",  # Уникальный код товара.
    "url": "str",  # Ссылка на страницу товара.
    "title": "str",  # Заголовок/название товара (! Если в карточке товара указан цвет или объем, но их нет в названии, необходимо добавить их в title в формате: "{Название}, {Цвет или Объем}").
    "marketing_tags": ["str"],  # Список маркетинговых тэгов, например: ['Популярный', 'Акция', 'Подарок']. Если тэг представлен в виде изображения собирать его не нужно.
    "brand": "str",  # Бренд товара.
    "section": ["str"],  # Иерархия разделов, например: ['Игрушки', 'Развивающие и интерактивные игрушки', 'Интерактивные игрушки'].
    "price_data": {
        "current": float,  # Цена со скидкой, если скидки нет то = original.
        "original": float,  # Оригинальная цена.
        "sale_tag": "str"  # Если есть скидка на товар то необходимо вычислить процент скидки и записать формате: "Скидка {discount_percentage}%".
    },
    "stock": {
        "in_stock": bool,  # Есть товар в наличии в магазине или нет.
        "count": int  # Если есть возможность получить информацию о количестве оставшегося товара в наличии, иначе 0.
    },
    "assets": {
        "main_image": "str",  # Ссылка на основное изображение товара.
        "set_images": ["str"],  # Список ссылок на все изображения товара.
        "view360": ["str"],  # Список ссылок на изображения в формате 360.
        "video": ["str"]  # Список ссылок на видео/видеообложки товара.
    },
    "metadata": {
        "__description": "str",  # Описание товара
        "KEY": "str",
        "KEY": "str",
        "KEY": "str"
        # Также в metadata необходимо добавить все характеристики товара которые могут быть на странице.
        # Например: Артикул, Код товара, Цвет, Объем, Страна производитель и т.д.
        # Где KEY - наименование характеристики.
    }
    "variants": int,  # Кол-во вариантов у товара в карточке (За вариант считать только цвет или объем/масса. Размер у одежды или обуви варинтами не считаются).
}
```

## Инструкция по запуску парсера
* Сделать Fork - (https://github.com/Lif-a-nova/fix-price-parser)
* Клонировать свой репозиторий
```
git clone git@github.com:....
```
* Установить и активировать окружение
```
cd fix_price
python -m venv scrapy_venv
source scrapy_venv/Scripts/activate
```
* Установить зависимости
```
pip install -r requirements.txt
```
* Запустить парсер и сохранить данные
```
cd fix-price/spiders/
scrapy crawl category -o product.json
```
* Не забыть:
```
Пользоваться с удовольствием!
```
