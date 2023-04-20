DropDownList Sorter for Yii2
============================

[![Latest Stable Version](https://poser.pugx.org/i4erkasov/yii2-drop-down-list-sorter/v/stable)](https://packagist.org/packages/i4erkasov/yii2-drop-down-list-sorter)
[![Total Downloads](https://poser.pugx.org/i4erkasov/yii2-drop-down-list-sorter/downloads)](https://packagist.org/packages/i4erkasov/yii2-drop-down-list-sorter)
[![License](https://poser.pugx.org/i4erkasov/yii2-drop-down-list-sorter/license)](https://packagist.org/packages/i4erkasov/yii2-drop-down-list-sorter)

Yii2 widget to sort ActiveDataProvider data using a drop-down list.


Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Run the following command

```
php composer.phar require --prefer-dist i4erkasov/yii2-drop-down-list-sorter "*"
```

or add the following to the require section of your composer.json file:

```
"i4erkasov/yii2-drop-down-list-sorter": "*"
```

to the require section of your `composer.json` file.


Usage
-----

Once the extension is installed, simply use it in your code by  :

Add to our ActiveDataProvider in sort::
```php
$dataProvider = new yii\data\ActiveDataProvider([
            'query'      => $query,
            'pagination' => [
                'pageSize'      => 32,
                'route'         => '/catalog/',
                'pageSizeParam' => false,
            ],
            'sort'       => new \i4erkasov\dropdownlistsorter\data\Sort([
                'route'        => '/catalog/',
                'unsetParams'  => ['page', '_pjax'],
                'attributes'   => [
                    'index' => [
                        'asc'   => ['sort_index' => SORT_ASC],
                        'desc'  => false,
                        'label' => ['asc' => Yii::t('app', 'default')],
                    ],
                    'name'  => [
                        'asc'   => ['name' => SORT_ASC],
                        'desc'  => false,
                        'label' => ['asc' => Yii::t('app', 'by name')],
                    ],
                    'price' => [
                        'asc'   => ['price' => SORT_ASC],
                        'desc'  => ['price' => SORT_DESC],
                        'label' => [
                            'asc'  => Yii::t('app', 'price asc'),
                            'desc' => Yii::t('app', 'price desc'),
                        ],
                    ],
                ],
                'defaultOrder' => [
                    'index' => SORT_ASC,
                ],
            ]),
        ]);
```

The set of parameters is the same as for standard [Yii2 Sort](https://www.yiiframework.com/doc/api/2.0/yii-data-sort)

For exclusively:

The defaultOrder parameter is required
And
Added the unsetParams parameter for exclude GET parameters, as shown above when generating the sort URL.

Example:
```php
'unsetParams'  => ['page', '_pjax'],
```

php example:
```php
echo \i4erkasov\dropdownlistsorter\widget\DropDownSorter::widget([
    'sort'       => $dataProvider->sort,
    'class'      => 'filter-select__select',
    'onchange'   => '$.pjax.reload({container: "#pjax-catalog", url: $(this).val()})',
    'attributes' => [
        'index',
        'name',
        'price',
    ],
]);
```

yii2-twig example:
```php
{{ use('i4erkasov/dropdownlistsorter/widget/DropDownSorter') }}
{{ use('yii/widgets/Pjax') }}

{{ dropDownSorter_widget({
    'sort': dataProvider.sort,
    'options': {
        'class': 'filter-select__select',
        'onchange': '$.pjax.reload({container: "#pjax-catalog", url: $(this).val()})',
    },
    'attributes': [
        'index',
        'name',
        'price'
    ]
}) | raw }}

{{ pjax_end() }}

```

>**Note**
Please note that in the above examples, the handling of the event for "change" DropDownList is implemented via pjax
```php
'onchange': '$.pjax.reload({container: "#pjax-catalog", url: $(this).val()})',
```
## License
This package is released under the MIT License. See LICENSE.md for details.

## Contributing
You can contribute by submitting pull requests or creating new issues.
