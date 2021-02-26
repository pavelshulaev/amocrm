# afterMakeCatalogElementByBasketItem (с версии 2.6.9)
Вызывается после созданием товара для амо на основе товара из корзины. Есть возможность подменить созданный товар своим, либо обнулить результаты создания, вернув `false`.

## Параметры
0. `Bitrix\Sale\BasketItem` — товар в корзине;
1. `AmoCRM\Models\CatalogElementModel` — созданный товар.

## Обработка события
Аналогично обработке [onBeforePushData](./onbeforepushdata.md).