---
layout: single
date:   2017-12-23 14:53:09
title:  "用PHP实践TDD ——Before Training"
header:
  overlay_image: http://7xil47.com1.z0.glb.clouddn.com/common_title_tdd.jpg
  overlay_filter: 0.5
categories:
  - Development
  - TDD
toc: true
toc_label: 目录
toc_icon: fa-gears
---

需求：“购物车能够计算已添加的商品总价格，当商品的单价大于等于100时，该商品优惠50元”

## 测试用例分析

根据需求，抽象出如下几个测试用例：

* 购物车有且仅有一个商品，该商品价格为150，总价为（150 - 50）= 100
* 购物车有且仅有一个商品，该商品价格为100，总价为（100 - 50）= 50
* 购物车有且仅有一个商品，该商品价格为80，总价为80
* 购物车有三件商品，价格分别是150、100、100，总价为（150 - 50）+（100 - 50）+（100 - 50）= 200
* 购物车有三件商品，价格分别是150、100、80，总价为（150 - 50）+（100 - 50）+ 80 = 230
* 购物车有三件商品，价格分别是90、80、70，总价为 90 + 80 + 70 = 240

## 完成初步设计

考虑设计如下三个类：
* 商品类
* 优惠类
* 购物车类

大致的类功能和关系如下图：

![类图](http://7xil47.com1.z0.glb.clouddn.com/2017-12-23-experience-tdd-using-php-class-diagram.png)

购物车内商品的总价格将会由`Cart`类的`getPrice`方法完成。

## 编写测试用例

根据设计思路，创建6个TestCase，其运行一定会失败。大致代码如下：

```php
<?php
/**
 * Created by PhpStorm.
 * User: zhaoshukai
 * Date: 2017-12-23
 * Time: 12:24
 */
use PHPUnit\Framework\TestCase;

/**
 * Class CartTest
 */
class CartTest extends TestCase
{
    /**
     * 购物车有且仅有一个商品，该商品价格为150，总价为（150 - 50）= 100
     */
    public function testOneItemMoreThanOneHundred() {
        $cart    = new Cart();
        $this->assertEquals(100,$cart->getPrice());
    }
    /**
     * 购物车有且仅有一个商品，该商品价格为100，总价为（100 - 50）= 50
     */
    public function testOneItemEqualToOneHundred() {
        $cart    = new Cart();
        $this->assertEquals(50,$cart->getPrice());
    }
    /**
     * 购物车有且仅有一个商品，该商品价格为80，总价为80
     */
    public function testOneItemLessThanOneHundred() {
        $cart    = new Cart();
        $this->assertEquals(80,$cart->getPrice());
    }
    /**
     * 购物车有三件商品，价格分别是150、100、100，总价为（150 - 50）+（100 - 50）+（100 - 50）= 200
     */
    public function testMutiItemesAllMoreThanOneHundred() {
        $cart    = new Cart();
        $this->assertEquals(200,$cart->getPrice());
    }
    /**
     * 购物车有三件商品，价格分别是150、100、80，总价为（150 - 50）+（100 - 50）+ 80 = 230
     */
    public function testMutiItemesSomeMoreThanOneHundred() {
        $cart    = new Cart();
        $this->assertEquals(230,$cart->getPrice());
    }
    /**
     * 购物车有三件商品，价格分别是90、80、70，总价为 90 + 80 + 70 = 240
     */
    public function testMutiItemesAllLessThanOneHundred() {
        $cart    = new Cart();
        $this->assertEquals(240,$cart->getPrice());
    }
}
```

## 开发类实现代码

将上图中的设计分别生成实现代码：

**商品类**

```php
<?php
/**
 * Created by PhpStorm.
 * User: zhaoshukai
 * Date: 2017-12-23
 * Time: 12:08
 */
namespace ExperienceTDD;

class Product
{
    var $price;

    public function setPrice($price) {
        $this->price = $price;
    }

    public function getPrice() {
        return $this->price;
    }
}
```

**购物车类**

```php
<?php
/**
 * Created by PhpStorm.
 * User: zhaoshukai
 * Date: 2017-12-23
 * Time: 12:10
 */
namespace ExperienceTDD;
require_once __DIR__ . '/Discount.php';

class Cart
{
    var $productList;

    function addProduct($product) {
        $this->productList[] = $product;
    }

    function getPrice() {
        $totalPrice = 0;
        foreach($this->productList as $product) {
            $totalPrice += Discount::price($product->getPrice());
        }

        return $totalPrice;
    }
}
```

**优惠类**

```php
<?php
/**
 * Created by PhpStorm.
 * User: zhaoshukai
 * Date: 2017-12-23
 * Time: 12:09
 */
namespace ExperienceTDD;

class Discount
{
    static function price($price) {
        if($price >= 100) {
            $price -= 50;
        }

        return $price;
    }

}
```

## 完善测试用例

运行上述6个TestCase，并根据结果调整实现代码，保证用例全部运行成功通过，结果代码如下：

```php
<?php
/**
 * Created by PhpStorm.
 * User: zhaoshukai
 * Date: 2017-12-23
 * Time: 12:24
 */
use PHPUnit\Framework\TestCase;

require_once __DIR__.'/../src/ExperienceTDD/Product.php';
require_once __DIR__.'/../src/ExperienceTDD/Cart.php';

/**
 * Class CartTest
 */
class CartTest extends TestCase
{
    /**
     * 购物车有且仅有一个商品，该商品价格为150，总价为（150 - 50）= 100
     */
    public function testOneItemMoreThanOneHundred() {
        $product = new \ExperienceTDD\Product();
        $cart    = new \ExperienceTDD\Cart();

        $product->setPrice(150);
        $cart->addProduct($product);

        $this->assertEquals(100,$cart->getPrice());
    }
    /**
     * 购物车有且仅有一个商品，该商品价格为100，总价为（100 - 50）= 50
     */
    public function testOneItemEqualToOneHundred() {
        $product = new \ExperienceTDD\Product();
        $cart    = new \ExperienceTDD\Cart();

        $product->setPrice(100);
        $cart->addProduct($product);

        $this->assertEquals(50,$cart->getPrice());
    }
    /**
     * 购物车有且仅有一个商品，该商品价格为80，总价为80
     */
    public function testOneItemLessThanOneHundred() {
        $product = new \ExperienceTDD\Product();
        $cart    = new \ExperienceTDD\Cart();

        $product->setPrice(80);
        $cart->addProduct($product);

        $this->assertEquals(80,$cart->getPrice());
    }
    /**
     * 购物车有三件商品，价格分别是150、100、100，总价为（150 - 50）+（100 - 50）+（100 - 50）= 200
     */
    public function testMutiItemesAllMoreThanOneHundred() {
        $product  = new \ExperienceTDD\Product();
        $product2 = new \ExperienceTDD\Product();
        $product3 = new \ExperienceTDD\Product();
        $cart     = new \ExperienceTDD\Cart();

        $product->setPrice(150);
        $product2->setPrice(100);
        $product3->setPrice(100);
        $cart->addProduct($product);
        $cart->addProduct($product2);
        $cart->addProduct($product3);

        $this->assertEquals(200,$cart->getPrice());
    }
    /**
     * 购物车有三件商品，价格分别是150、100、80，总价为（150 - 50）+（100 - 50）+ 80 = 230
     */
    public function testMutiItemesSomeMoreThanOneHundred() {
        $product  = new \ExperienceTDD\Product();
        $product2 = new \ExperienceTDD\Product();
        $product3 = new \ExperienceTDD\Product();
        $cart     = new \ExperienceTDD\Cart();

        $product->setPrice(150);
        $product2->setPrice(100);
        $product3->setPrice(80);
        $cart->addProduct($product);
        $cart->addProduct($product2);
        $cart->addProduct($product3);

        $this->assertEquals(230,$cart->getPrice());
    }
    /**
     * 购物车有三件商品，价格分别是90、80、70，总价为 90 + 80 + 70 = 240
     */
    public function testMutiItemesAllLessThanOneHundred() {
        $product  = new \ExperienceTDD\Product();
        $product2 = new \ExperienceTDD\Product();
        $product3 = new \ExperienceTDD\Product();
        $cart     = new \ExperienceTDD\Cart();

        $product->setPrice(90);
        $product2->setPrice(80);
        $product3->setPrice(70);
        $cart->addProduct($product);
        $cart->addProduct($product2);
        $cart->addProduct($product3);

        $this->assertEquals(240,$cart->getPrice());
    }
}
```

实践项目完整源代码下载：[项目源代码](https://github.com/shukai0828/ExperienceTDDUsingPHP/archive/accomplishment.zip)
