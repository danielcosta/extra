# Marketplace Extra.com.br #
API PHP para integração com extra.com.br

* [Autenticação](#autenticação)
* [Serviços para consulta de categorias](#serviços-para-consulta-de-categorias)
    - [Recupera uma lista de categorias](#recupera-uma-lista-de-categorias)
    - [Recupera detalhes de uma categoria informada.](#recupera-detalhes-de-uma-categoria-informada)

* [Serviços de itens vendidos pelo lojista](#serviços-de-itens-vendidos-pelo-lojista)
    - [Recupera itens do Lojista.](#recupera-itens-do-lojista)
    - [Recupera Itens do Lojista que já estão disponíveis para venda.](#recupera-itens-do-lojista-que-já-estão-disponíveis-para-venda)
    - [Recupera detalhes do Item do Lojista através do sku informado.](#recupera-detalhes-do-item-do-lojista-através-do-sku-informado)
    - [Recupera detalhes do Item do Lojista através do sku do lojista.](#recupera-detalhes-do-item-do-lojista-através-do-sku-do-lojista)
    - [Serviço utilizado para registrar Itens do Lojista que já estão disponíveis para venda.](#serviço-utilizado-para-registrar-itens-do-lojista-que-já-estão-disponíveis-para-venda)
    - [Serviço de atualização de preços.](#serviço-de-atualização-de-preços)
    - [Atualiza a quantidade disponível para venda do Item do Lojista informado.](#atualiza-a-quantidade-disponível-para-venda-do-item-do-lojista-informado)

## Instalação Composer ##

1. Baixe o [`composer.phar`](https://getcomposer.org/composer.phar).
2. Adiciona o pacote `"luzpropria/extra": "dev-master"` no seu composer.json
3. Execute o composer `php composer.phar install` ou `php composer.phar update`

* Problemas com codificação utilizar URF-8 `header('Content-Type: text/html; charset=utf-8');`

### Autenticação ###
```php
use LuzPropria\Extra\Autenticacao\Autenticacao;
use LuzPropria\Extra\Enviar\Extra;

$auth = new Autenticacao();
$auth->setAuthToken('nova-auth-token');
$auth->setAppToken('nova-app-token');
// Desenvolvimento `sandbox` ou Produção (default) `producao`.
$auth->setSandbox('sandbox');
$extra = new Extra($auth);

```

### Serviços para consulta de categorias ###

###### Recupera uma lista de categorias ######
```php
use LuzPropria\Extra\Api\Categorie\Categories;
use LuzPropria\Extra\Api\Categorie\Response\Category;
use LuzPropria\Extra\Utils\ArrayCollection;
use LuzPropria\Extra\Enviar\Exception\Exception as LPException;

$class = new Categories();
$class->setOffset(0); // Parâmetro utilizado para limitar a quantidade de registros trazidos por página.
$class->setLimit(50); // Parâmetro utilizado para limitar a quantidade de registros trazidos pela operação.

try {
    /** @var ArrayCollection $retorno */
    $retorno = $extra->send($class);

} catch (LPException $e ) {
    $e->getMessage();
}

if($retorno instanceof ArrayCollection) {
    /** @var Category $categoria */
    foreach($retorno as $categoria) {
        var_dump($categoria);
    }
}
```

###### Recupera detalhes de uma categoria informada. ######
```php
use LuzPropria\Extra\Api\Categorie\CategoriesLevelId;
use LuzPropria\Extra\Api\Categorie\Response\Category;
use LuzPropria\Extra\Enviar\Exception\Exception as LPException;

$class = new CategoriesLevelId();
$class->setLevelId(80056); // Id da categoria

try {
    /** @var Category $retorno */
    $retorno = $extra->send($class);

    var_dump($retorno);
} catch (LPException $e ) {
    $e->getMessage();
}
```
### Serviços de itens vendidos pelo lojista ###

###### Recupera itens do Lojista. ######
```php
use LuzPropria\Extra\Api\Seller\SellerGetItems;
use LuzPropria\Extra\Api\Seller\Response\SellerItem;
use LuzPropria\Extra\Utils\ArrayCollection;
use LuzPropria\Extra\Enviar\Exception\Exception as LPException;

$class = new SellerGetItems();
$class->setOffset(0); // Parâmetro utilizado para limitar a quantidade de registros trazidos por página.
$class->setLimit(50); // Parâmetro utilizado para limitar a quantidade de registros trazidos pela operação.

try {
    /** @var ArrayCollection $retorno */
    $retorno = $extra->send($class);

} catch (LPException $e ) {
    $e->getMessage();
}

if($retorno instanceof ArrayCollection) {
    /** @var SellerItem $selleritem */
    foreach($retorno as $selleritem) {
        var_dump($selleritem);
    }
}
```

###### Recupera Itens do Lojista que já estão disponíveis para venda. ######
```php
use LuzPropria\Extra\Api\Seller\SellerItemsStatusSelling;
use LuzPropria\Extra\Api\Seller\Response\SellerItem;
use LuzPropria\Extra\Utils\ArrayCollection;
use LuzPropria\Extra\Enviar\Exception\Exception as LPException;

$class = new SellerItemsStatusSelling();
$class->setOffset(0); // Parâmetro utilizado para limitar a quantidade de registros trazidos por página.
$class->setLimit(50); // Parâmetro utilizado para limitar a quantidade de registros trazidos pela operação.

try {
    /** @var ArrayCollection $retorno */
    $retorno = $extra->send($class);

} catch (LPException $e ) {
    $e->getMessage();
}
if($retorno instanceof ArrayCollection) {
    /** @var SellerItem $selleritem */
    foreach($retorno as $selleritem) {
        var_dump($selleritem);
    }
}
```

###### Recupera detalhes do Item do Lojista através do sku informado. ######
```php
use LuzPropria\Extra\Api\Seller\SellerGetItem;
use LuzPropria\Extra\Api\Seller\Response\SellerItem;
use LuzPropria\Extra\Enviar\Exception\Exception as LPException;

$class = new SellerGetItem();
$class->setSkuId(21956411); // SKU ID do produto no Marketplace.

try {
    /** @var SellerItem $retorno */
    $retorno = $extra->send($class);

    var_dump($retorno);
} catch (LPException $e ) {
    $e->getMessage();
}
```

###### Recupera detalhes do Item do Lojista através do sku do lojista. ######
```php
use LuzPropria\Extra\Api\Seller\SellerItemsSkuOrigin;
use LuzPropria\Extra\Api\Seller\Response\SellerItem;
use LuzPropria\Extra\Enviar\Exception\Exception as LPException;

$class = new SellerItemsSkuOrigin();
$class->setSkuOrigin(848); // SKU ID do produto do Lojista.

try {
    /** @var SellerItem $retorno */
    $retorno = $extra->send($class);

    var_dump($retorno);
} catch (LPException $e ) {
    $e->getMessage();
}
```

###### Serviço utilizado para registrar Itens do Lojista que já estão disponíveis para venda. ######
```php
use LuzPropria\Extra\Api\Seller\SellerItem;
use LuzPropria\Extra\Api\Seller\SellerItems;
use LuzPropria\Extra\Api\Seller\Response\SellerCreate;
use LuzPropria\Extra\Enviar\Exception\NotCreateException;
use LuzPropria\Extra\Enviar\Exception\ParametersInvalidException;
use LuzPropria\Extra\Enviar\Exception\Exception as LPException;

$SellerItem = new SellerItem();
$SellerItem
    ->setSkuId(10) // SKU ID do produto do Lojista `Opcional`
    ->setSkuOrigin('1') // SKU ID do produto no Marketplace
    ->setDefaultPrice(11.10) // Preço “de” no Marketplace
    ->setSalePrice(11.10) // Preço “por”. Preço real de venda
    ->setAvailableQuantity(5) // Quantidade disponível para venda
    ->setTotalQuantity(8) // Quantidade disponível em estoque
    ->setCrossDockingTime(1) // Tempo de fabricação `Opcional`
;

$class = new SellerItems();
$class->setSellerItem($SellerItem); // Ítem do Lojista

try {
    /** @var SellerCreate $retorno */
    $retorno = $extra->send($class);

    var_dump($retorno);
} catch (ParametersInvalidException $e) {
    echo $e->getMessage(); // Parametros invalido
} catch (NotCreateException $e) {
    echo $e->getMessage(); // Retorno erro no envio
} catch (LPException $e ) {
    echo $e->getMessage(); // Erro não detectado
}
```

###### Serviço de atualização de preços. ######
Atualiza o preço ´de´ e o preço ´por´ (preço real para venda) do Item do Lojista informado.

```php
use LuzPropria\Extra\Api\Seller\PriceUpdate;
use LuzPropria\Extra\Api\Seller\SellerItemsPrices;
use LuzPropria\Extra\Enviar\Exception\NotUpdateException;
use LuzPropria\Extra\Enviar\Exception\ParametersInvalidException;
use LuzPropria\Extra\Enviar\Exception\Exception as LPException;

$priceUpdate = new PriceUpdate();
$priceUpdate
    ->setDefaultPrice(20.90) // Preço ´de´
    ->setSalePrice(20.00) // Preço ´por´
;
$class = new SellerItemsPrices();
$class
    ->setSkuId(21956924) // SKU ID do produto no Marketplace.
    ->setPriceUpdate($priceUpdate) // Objeto priceUpdate.
;

try {

    $resturn = $extra->send($class);

    // Atualizado.
    var_dump($resturn);

} catch (ParametersInvalidException $e) {
    echo $e->getMessage(); // Parametros invalido
} catch (NotUpdateException $e ) {
    echo $e->getMessage(); // Não foi possivel atualizar
} catch (LPException $e ) {
    echo $e->getMessage(); // Erro não detectado
}
```

###### Atualiza a quantidade disponível para venda do Item do Lojista informado. ######

```php
use LuzPropria\Extra\Api\Seller\StockUpdate;
use LuzPropria\Extra\Api\Seller\SellerItemsStock;
use LuzPropria\Extra\Enviar\Exception\NotUpdateException;
use LuzPropria\Extra\Enviar\Exception\ParametersInvalidException;
use LuzPropria\Extra\Enviar\Exception\Exception as LPException;

$stockUpdate = new StockUpdate();
$stockUpdate
    ->setAvailableQuantity(2) // Quantidade disponível para venda
    ->setTotalQuantity(4) // Quantidade disponível em estoque
;
$class = new SellerItemsStock();
$class
    ->setSkuId(21956924) // SKU ID do produto no Marketplace.
    ->setStockUpdate($stockUpdate) // Objeto stockUpdate.
;

try {

    $resturn = $extra->send($class);

    // Atualizado.
    var_dump($resturn);

} catch (ParametersInvalidException $e) {
    echo $e->getMessage(); // Parametros invalido
} catch (NotUpdateException $e ) {
    echo $e->getMessage(); // Não foi possivel atualizar
} catch (LPException $e ) {
    echo $e->getMessage(); // Erro não detectado
}
```

Desenvolvimento Rogério Adriano - [`LuzPropria`](http://www.luzpropria.com).