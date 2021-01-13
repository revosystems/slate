# Overview

## Prerequisistes

To be able to use the external api you need a `revo xef` account and a `access token`

1. Request an organization token at support@revo.works 

## Basic usage
The main URL for this api is


`http://overview.revo.works/api`

And you should provide the mandatory headers for the authentication


Header        | Value
--------------|----------
username      | {organization-name}
Authorization | Bearer {the-token}
Accept        | application/json

## Create lead
Create a lead end point.

`POST leads` 

`http://overview.revo.works/api/leads`

> Following fields can be passed (all of them are optional):

```sh
{
    'user_id'                   : null,  // unsignedInteger, operator_id if known
    'probability'               : null,  // tinyInteger [1 => VERY_LOW, 2 => LOW, 3 => MEDIUM, 4 => HIGH, 5 => VERY_HIGH]
    'total'                     : 0.0,  // decimal
    'total_devices'             : 10,  // decimal
    'trade_name'                : "Testing business",  // string
    'channel'                   : "thoughApi", // string (the way to know where the request comes from) (OPTIONAL) 
    'fiscal_name'               : "Testinig INC.", // stringº
    'name'                      : "John",  // string
    'cif'                       : "39393938",  // string
    'surname1'                  : "Snow",  // string
    'surname2'                  : "White",  // string
    'email'                     : "john@revo.works",  // string
    'phone'                     : "+34 668686854",  // varchar(255)
    'city'                      : "Manchester",  // string
    'product'                   : 2,  // tinyInteger [1 => Xef, 2 => Retail]
    'type_segment'              : 1,  // [1 => SEGMENT_SMALL, 2 => SEGMENT_MEDIUM, 3 => SEGMENT_LARGE]
    'general_typology'          : 1,  // GENERAL TYPOLOGIES IDS ARE SPECIFIED BELOW*
    'specific_typology'         : [1, 2] // SPECIFIC TYPOLOGIES IDS ARE SPECIFIED BELOW**
    'property_spaces'           : 4,  // PROPERTY SPACES IDS ARE  SPECIFIED BELOW***
    'property_quantity'         : 3,  // How many properties does the lead have
    'property_capacity'         : 99,  // Local Max capacity
    'property_franchise'        : 1,  // Is a franchise
    'devices'                   : 6,  // How many devices has?. integer
    'devices_current'           : "",  // Specify device names 
    'xef_pos_critical_quantity' : 4,  // Nº de comanderos entorno crítico. integer
    'xef_cash_quantity'         : 1,  // Cashier number. integer
    'xef_printers_quantity'     : 1,  // Kitchen printers. integer
    'xef_kds'                   : 1,  // Would likes KDS? boolean
    'xef_kds_quantity'          : 1,  // How many KDS? integer
    'pos'                       : -2,  // Current POS. -2 means None. tinyInteger
    'pos_other'                 : "",  // If -1 one specified, here an other pos name can be written. string
    'retail_sale_mode'          : 1, // Require a sale in front of customer? boolean
    'retail_sale_location'      : 2 // [1 => onLocal, 2 => onMobility] Sale location. bool
    'can_use_another_pos'       : 0,  // tinyInteger
    'xef_pms'                   : -1,  // Current PMS. -1 means Other. tinyInteger
    'xef_pms_other'             : "",  // If -1 one specified, here an other pms name can be written. string
    'erp'                       : -2,  // Current ERP. -2 means None. tinyInteger
    'erp_other'                 : "",  // If -1 one specified, here an other erp name can be written. string
    'soft_other'                : "",  // If -1 one specified, here an other soft name can be written. string
    'status'                    : 1,  // tinyInteger [1 => NEW, 2 => FIRST_CONTACT, 3 => VISITED, 4 => COMPLETED, 5 => FAILED]
    'offer_status'              : 1, // tinyInteger [1 => PENDING, 2 => IN_CURSE, 3 => PRESENTED, 4 => COMPLETED, 5 => EXPIRED]
    'soft'                  => [ -2 ],  Current SOFTS. -2 means None. array of tinyIntegers SOFTS IDS ARE SPECIFIED BELOW****
}
```

Response is a json with created lead:

`{"id": 1, "status": 0, ...}`

> Some Organizations can have custom fields, like it is shown in the following example:

```sh
{
    'is_already_customer'               : 1, // boolean
    'is_already_using_organization_tpv' : 0, // boolean
    'share_bandwith_with_customers'     : 2, // tinyInteger [1 => 'yes', 0 => 'no', 3 => 'yesButDontKnowMega' ]
    'is_smart_business_wired'           : 1, // tinyInteger [1 => 'yes', 0 => 'no', 3 => 'dontKnow' ]
}
```


### Tips


#### \* General typologies

Key | Value          
------------------|--------------
1 | Cafetería
2 | Bar
3 | Restaurante
4 | Discoteca
5 | Take away
6 | Delivery   
7 | Hotel      
9 | Panadería  
10 | Foodtruck  
11 | Comida al peso
12 | Solo eventos
13 | Retail store
14 | Distribución de productos y Fuerza comercial
15 | Autoventa / movilidad
16 | Peluquería / estética
17 | Panadería 
18 | Venta a granel
19 | Aviación / transporte
20 | Eventos / corners
21 | Espectáculos


#### ** Specific typologies
        
Key | Value          
------------------|--------------
1 | Carta
2 | Eventos
3 | Menú
4 | Retail store
5 | Distribución de productos y Fuerza comercial
6 | Autoventa / movilidad
7 | Peluquería / estética
8 | Panadería
9 | Venta a granel
10 | Aviación / transporte
11 | Eventos / corners
12 | Espectáculos


#### *** Property spaces

Key | Value          
------------------|--------------
1 | Una única Sala
2 | Sala + Terraza
3 | Multiples Salas y/o Terrazas
4 | Estándar
5 | Diferentes áreas / plantas
6 | Locales separados


#### **** Softs

Key | Value          
------------------|--------------
1 | Gstock
2 | CHef Control
3 | Distrito K
4 | GIRnet
5 | Revo Flow
6 | CoverManager
7 | ElTenedor
8 | Restoo
9 | Deliveroo
10 | Glovo
11 | tSpoonLab
12 | GIRnet
13 | PrestaShop
14 | WooCommerce


## Responses

Value | Meaning                    | Description
------|----------------------------|------------------------------------------------
201   | HTTP_CREATED               | Call succeed
422   | HTTP_UNPROCESSABLE_ENTITY  | Call failed
