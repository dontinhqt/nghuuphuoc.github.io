---
layout: post
title:  "Quy tắc viết mã nguồn PHP chuẩn [Vietnamese]"
date:   2013-10-10 23:54:20
---

Tài liệu này quy định *cách viết mã PHP chuẩn* được tổng hợp dựa trên các nguồn, quy chuẩn sau:

* Chuẩn _autoload_, còn biết đến là [PSR-0](http://www.php-fig.org/psr/0/)
* Chuẩn viết mã [PSR-1](http://www.php-fig.org/psr/1/)
* Hướng dẫn cách viết mã chuẩn [PSR-2](http://www.php-fig.org/psr/2/)

PSR viết tắt cho _PHP Specification Request_, bao gồm các quy chuẩn được nhiều thành viên của nhóm FIG (_Framework Interop Group_) đề xuất. Nhóm này
tập hợp nhiều nhà lập trình viên, kiến trúc sư nổi tiếng của các dự án, sản phẩm PHP lớn như [Zend Framework](http://framework.zend.com),
[Symfony](http://www.symfony.com), [CakePHP](http://cakephp.org), [Joomla](http://www.joomla.org), [Drupal](http://drupal.org), ...

Mục tiêu của nhóm là xây dựng nên một quy chuẩn để các thư viện, framework của PHP có thể tuân theo 1 chuẩn duy nhất, và từ đó có thể hoạt động được với nhau dễ dàng hơn.
Bạn có thể xem thêm thông tin về nhóm này trên website <http://www.php-fig.org>.

Mặc dù PSR không phải là quy chuẩn chính thức của PHP, nhưng nó đã và đang được khuyến khích, sử dụng bởi nhiều thư viện, framework, sản phẩm PHP lớn.

Ngoài ra, quy chuẩn trong tài liệu này cũng tham khảo:

* [Quy định viết mã chuẩn của Symfony](http://symfony.com/doc/current/contributing/code/standards.html)
* [Quy định viết mã chuẩn của Zend Framework](http://framework.zend.com/manual/en/coding-standard.overview.html)

## Files

### Thẻ PHP

* Mã PHP PHẢI sử dụng cặp thẻ đầy đủ ```<?php ?>```. NÊN sử dụng cặp thẻ ngắn ```<?= ?>``` dành cho phần hiển thị (thay cho hàm ```echo```) nếu nó được cho phép.

Các cặp thẻ ngắn ```<?``` và ```<?=``` có thể không được kích hoạt nếu ```short_open_tag = Off``` được thiết lập.
Tuy nhiên, từ phiên bản PHP 5.4 trở đi, ```<?=``` luôn được kích hoạt.

* Thẻ đóng ```?>``` ở cuối file PHẢI bị xoá nếu file chỉ chứa duy nhất mã PHP.

### Định dạng file

* File PHP PHẢI kết thúc bằng một dòng trắng
* File PHP PHẢI sử dụng định dạng cuối dòng Unix LF
* File PHP PHẢI sử dụng mã UTF-8 (không có BOM)

BOM (_Byte Order Mark_) là chuỗi các byte EF BB BF ở đầu file cho phép phần mềm biết một file là UTF-8.
Nó được khuyến cáo không nên sử dụng bởi [chuẩn Unicode](http://www.unicode.org/versions/Unicode5.0.0/ch02.pdf), mục 2.6.

### Nội dung file

Mỗi một file PHP chỉ NÊN chứa một trong 2 kiểu nội dung sau mà KHÔNG NÊN bao gồm cả 2:

* Khai báo lớp, hàm, hằng số, ...
* Những kiểu thực thi không liên quan đến việc khai báo hàm, lớp, hằng số, ví dụ: hiển thị nội dung, tải file khác (```include```), thay đổi thiết lập ```ini```,
đọc nội dung từ file hoặc lưu file, ...

Đoạn mã sau bao gồm cả 2 kiểu nội dung trên và vì thế được coi là không theo chuẩn:

```php
<?php
// Thay đổi thiết lập ini
ini_set('error_reporting', E_ALL);

// Tải file
include "file.php";

// Hiển thị nội dung
echo "<html>\n";

// Khai báo hàm
function foo()
{
}
```

## Dòng

* Mỗi dòng KHÔNG NÊN quá 80 ký tự. Các dòng dài hơn NÊN được chia thành nhiều dòng có độ dài ít hơn 80 ký tự.
* Nếu dòng không phải là dòng trắng, KHÔNG NÊN có ký tự trắng ở cuối dòng.
* CÓ THỂ thêm các dòng trắng để mã nguồn dễ đọc hơn, hoặc để nhóm các đoạn mã trong cùng một khối.
* KHÔNG ĐƯỢC có nhiều hơn một câu lệnh trên một dòng.

## Thụt dòng

Mã nguồn PHẢI sử dụng 4 khoảng trắng cho mỗi lần thụt dòng, và KHÔNG ĐƯỢC sử dụng _tab_ cho việc thụt dòng.

Việc chỉ sử dụng khoảng trắng mà không sử dụng cả khoảng trắng và _tab_ giúp bạn tránh các vấn đề với quản lý phiên bản
mã nguồn khi sử dụng _diff_, _patch_, _history_.

![Cấu hình độ dài thụt dòng với PHP Storm](/img/phpstorm-indent.png)

Với PHP Storm, bạn có thể cấu hình độ dài khoảng trắng khi thụt dòng bằng cách vào _PHP Storm &rarr; Preferences_, chọn
_Code Style &rarr; PHP_ ở bên trái, chọn tab _Tabs and Indent_ ở bên phải, và nhập 4 vào các ô _Tab size_, _Indent_ và
_Continuation indent_ như hình dưới đây. Nhấn nút _Apply_.

## Tên namespace và tên lớp

Khái niệm _lớp_ ở đây bao gồm ```class```, ```interface``` và ```trait```.

__PHẢI sử dụng tiếng Anh__ để đặt tên namespace, lớp, biến, ...

### Namespace

* Với phiên bản PHP 5.3 và sau này, PHẢI sử dụng namespace khi định nghĩa lớp
* Tên đầy đủ của namespace và lớp PHẢI theo cấu trúc ```\<Tên Vendor>\(<Namespace>\)*<Tên lớp>```

Vendor thường là tên sản phẩm hoặc tên công ty, như là một định dạng để nhận biết và phân biệt tên lớp của bạn. Ví dụ Symfony, Zend, ...

Với APL, _vendor_ có thể là tên sản phẩm như Coo, Zo2, ...

* Mỗi namespace PHẢI có namespace gốc, thường là tên Vendor
* Mỗi namespace CÓ THỂ bao gồm tuỳ ý số lượng namespace con
* Phần ngăn cách namespace sẽ được thay thế bằng ```DIRECTORY_SEPARATOR``` khi muốn xác định đường dẫn của file định nghĩa namespace.

```php
<?php
// Zend Framework 2.x (sử dụng PHP 5.3+)
// Lớp này được định nghĩa ở file Zend/Mvc/Application.php
namespace Zend\Mvc;

class Application
{
}
```

### Lớp

* Tên lớp PHẢI được khai báo ở dạng ```StudlyCaps```
* PHẢI thêm tiếp đầu ngữ ```Abstract``` cho các lớp ```abstract```
* PHẢI thêm tiếp vị ngữ ```Interface``` cho các ```interface```
* PHẢI thêm tiếp vị ngữ ```Trait``` cho các ```trait```
* PHẢI thêm tiếp vị ngữ ```Exception``` cho các lớp kế thừa từ ```Exception```
* Với PHP phiên bản 5.2.x và trước đó, tên lớp PHẢI bắt đầu bằng ```Vendor_```
* Các ký tự ```_``` có trong tên lớp sẽ được thay thế bằng ```DIRECTORY_SEPARATOR``` khi muốn xác định đường dẫn đến file định nghĩa lớp.

```php
<?php
// Zend Framework 1.x (sử dụng PHP 5.2+)
// Lớp này được định nghĩa ở fle Zend/Db/Table.php
class Zend_Db_Table extends Zend_Db_Table_Abstract
{
}
```

Nhờ việc sử dụng cách đặt tên như trên, bạn có thể tải bất kỳ file nào bằng cách sử dụng hàm ```spl_autoload_register```.
Bạn không cần sử dụng các hàm ```require```, ```require_once``` để tải file khi gọi lớp nào đó.

### Khai báo ```namespace``` và ```use```

* Nếu sử dụng nhiều khai báo ```use```, chúng PHẢI được sắp xếp dựa trên tên namespace theo thứ tự ABC.

Kể từ phiên bản 6.0, PHP Storm tự động thêm khai báo ```use``` và sắp xếp các khai báo này theo thứ tự ABC.

* Các khai báo ```use``` sử dụng các class, namespace được PHP cung cấp sẵn (như ```stdClass```) PHẢI được đặt trước các khai báo ```use``` của các thư viện khác.
* PHẢI có một dòng trắng sau khai báo ```namespace```
* Các khai báo ```use```, nếu có, PHẢI sau khai báo ```namespace```
* Chỉ có duy nhất một từ khoá ```use``` cho mỗi khai báo sử dụng namespace
* PHẢI có một dòng trắng sau khai báo ```use``` cuối cùng

```php
<?php
namespace Zend\Json;

use stdClass;
use Zend\Json\Exception\InvalidArgumentException;
use Zend\Json\Exception\RuntimeException;

class Decoder
{
}
```

## Biến

_Sử dụng danh từ tiếng Anh_ để đặt tên biến.

### Chuỗi

* PHẢI sử dụng dấu nháy đơn (```'```) để khai báo chuỗi không chứa biến:

```php
<?php
$a = 'Example String';
```

* Với chuỗi không chứa biến, nhưng chứa dấu nháy đơn (```'```), sử dụng dấu nháy kép (```""```) để bọc chuỗi:

```php
<?php
$sql = "SELECT `id`, `name` from `people` "
     . "WHERE `name`='Fred' OR `name`='Susan'";
```

Định dạng này thường xuyên sử dụng cho câu lệnh SQL.

* Với chuỗi chứa biến, sử dụng một trong 2 định dạng sau:

```php
<?php
$greeting = "Hello $name, welcome back!";
$greeting = "Hello {$name}, welcome back!";
```

* Sử dụng phép toán ```.``` để nối chuỗi. PHẢI có một khoảng trắng trước và sau ```.```:

```php
<?php
// Trích từ Zend\Http\Request::renderRequestLine()
return $this->method . ' ' . (string) $this->uri . ' HTTP/' . $this->version;
```

Với chuỗi quá dài, NÊN tách chuỗi thành nhiều dòng. Trong trường hợp này, mỗi dòng mới bắt đầu bằng phép toán ```.```, được thụt vào ngang với phép toán ```=```:

```php
<?php
// Trích từ Zend\View\Helper\Gravatar::getAvatarUrl()
$src = $this->getGravatarUrl()
     . '/'   . md5($this->getEmail())
     . '?s=' . $this->getImgSize()
     . '&d=' . $this->getDefaultImg()
     . '&r=' . $this->getRating();
```

```php
<?php
// Trích từ Zend\Http\Response::toString()
$str  = $this->renderStatusLine() . "\r\n";
$str .= $this->getHeaders()->toString();
$str .= "\r\n";
$str .= $this->getContent();
```

### Mảng

* Với mảng có chỉ mục là số: PHẢI có một khoảng trắng sau mỗi dấu phẩy. KHÔNG ĐƯỢC có khoảng trắng trước mỗi dấu phẩy.

```php
<?php
$sampleArray = array(1, 2, 3, 'Zend', 'Studio');
```

CÓ THỂ tách các phần tử thành nhiều dòng. CÓ THỂ đặt nhiều phần tử trên một dòng. Sử dụng một trong 2 định dạng dưới đây:

```php
<?php
// Một số phần tử của mảng được đặt cùng dòng với tên mảng.
//
// Dòng thứ 2, 3, ... PHẢI được thụt vào ngang với dấu mở ngoặc đơn của dòng đầu tiên
$sampleArray = array(1, 2, 3, 'Zend', 'Studio',
                     $a, $b, $c,
                     56.44, $d, 500);
```

```php
<?php
// Tất cả phần tử của mảng được đặt trên dòng khác với tên mảng.
//
// Các dòng khai báo phần tử của mảng PHẢI được thụt vào 4 khoảng trắng so với
// phần khai báo mảng.
// Dấu ngoặc đóng của mảng PHẢI được đặt trên một dòng khác, thẳng hàng với
// phần khai báo mảng.
// NÊN thêm dấu phẩy ở cuối cùng ở tất cả các dòng chứa phần tử mảng.

namespace Zend\Mvc;

class Application implements
    ApplicationInterface,
    EventManagerAwareInterface
{
    protected $defaultListeners = array(
        'RouteListener',
        'DispatchListener',
        'ViewManager',
        'SendResponseListener',
    );
}
```

* Với mảng kết hợp (_associative array_): CÓ THỂ sử dụng một trong hai cách khai báo dưới đây.

```php
<?php
// Phần tử đầu tiên của mảng nằm cùng dòng với tên mảng.
//
// Dòng thứ 2, 3, ... PHẢI được thụt vào ngang với dấu mở ngoặc đơn của dòng đầu tiên.
// Các ký tự => PHẢI được đặt thẳng hàng với nhau
$sampleArray = array('firstKey'  => 'firstValue',
                     'secondKey' => 'secondValue');
```

```php
<?php
// Tất cả phần tử của mảng được đặt trên dòng khác với tên mảng.
//
// Các dòng khai báo phần tử của mảng PHẢI được thụt vào 4 khoảng trắng so với
// phần khai báo mảng.
// Dấu ngoặc đóng của mảng PHẢI được đặt trên một dòng khác, thẳng hàng với
// phần khai báo mảng.
// Các ký tự => PHẢI được đặt thẳng hàng với nhau.
// NÊN thêm dấu phẩy ở cuối cùng ở tất cả các dòng chứa phần tử mảng.

namespace Zend\Captcha;

class ReCaptcha extends AbstractAdapter
{
   protected $messageTemplates = array(
       self::MISSING_VALUE => 'Missing captcha fields',
       self::ERR_CAPTCHA   => 'Failed to validate captcha',
       self::BAD_CAPTCHA   => 'Captcha value is wrong: %value%',
   );
}
```

## Phép toán

* PHẢI có một khoảng trắng trước và sau phép toán
* Khi ép kiểu, PHẢI có một khoảng trắng sau dấu ngoặc đóng bọc kiểu của biến:

```php
<?php
namespace Zend\Filter;

class Callback extends AbstractFilter
{
    public function setCallbackParams($params)
    {
        $this->options['callback_params'] = (array) $params;
        return $this;
    }
}
```

## Lớp, thuộc tính và phương thức

Khái niệm _lớp_ ở đây bao gồm ```class```, ```interface``` và ```trait```.

### Kế thừa và thực thi

* Các từ khoá ```extends``` và ```implements``` PHẢI được đặt trên cùng một dòng với tên lớp.
* Dấu mở ngoặc nhọn cho tên lớp PHẢI được đặt ở dòng tiếp theo sau tên lớp. Dấu mở ngoặc nhọn đóng cho tên lớp PHẢI được
đặt ở dòng tiếp theo sau phần thân lớp.

```php
<?php
namespace Zend\Captcha;

abstract class AbstractAdapter extends AbstractValidator implements AdapterInterface
{
    ...
}
```

* Tên các _interface_ mà lớp thực thi CÓ THỂ được tách thành nhiều dòng, trong đó mỗi dòng con sẽ được thụt vào 4 khoảng trắng.
Trong trường hợp này, tên của _interface_ đầu tiên PHẢI được đặt ở dòng tiếp theo tên lớp, và mỗi một _interface_ PHẢI
được đặt trên một dòng.

```php
<?php
namespace Zend\Cache\Storage\Adapter;

class Memcached extends AbstractAdapter implements
    AvailableSpaceCapableInterface,
    FlushableInterface,
    TotalSpaceCapableInterface
{
    ...
}
```

### ```abstract```, ```final``` và ```static```

* Các từ khoá ```abstract``` và ```final``` PHẢI được đặt trước các từ khoá xác định mức độ truy cập (```public```, ```protected``` và ```private```)
* Từ khoá ```static``` PHẢI được đặt sau từ khoá xác định mức độ truy cập

```php
<?php
namespace Zend\Version;

final class Version
{
    protected static $latestVersion;

    public static function compareVersion($version)
    {
    }
    ...
}
```

### Hằng số

* Các hằng số ```true```, ```false``` và ```null``` PHẢI được viết ở dạng chữ thường
* Hằng số của lớp PHẢI được khai báo ở chữ in hoa, phân cách nhau bởi dấu gạch dưới ```_```
* Các hằng số chung một mục đích NÊN được đặt cùng một tiếp đầu ngữ. Dấu ```=``` để gán giá trị cho các hằng số này NÊN được đặt thẳng hàng với nhau

```php
<?php
namespace Zend\Authentication;

class Result
{
    const FAILURE                    =  0;
    const FAILURE_IDENTITY_NOT_FOUND = -1;
    const FAILURE_IDENTITY_AMBIGUOUS = -2;
    const FAILURE_CREDENTIAL_INVALID = -3;
    const FAILURE_UNCATEGORIZED      = -4;
    const SUCCESS                    =  1;
}
```

### Thuộc tính

* Tất cả thuộc tính PHẢI khai báo cấp độ truy cập (```public```, ```protected```, và ```private```).
Các từ khoá này được gọi là từ khoá _visibility_.
* KHÔNG ĐƯỢC sử dụng từ khoá ```var``` để khai báo thuộc tính.
Từ khoá ```var``` được sử dụng ở PHP 4, và có mức độ truy cập như ```public```.
* Tên các thuộc tính PHẢI được khai báo sử dụng định dạng ```camelCase```
* Mỗi thuộc tính PHẢI được khai báo trên một dòng.
* KHÔNG NÊN thêm gạch dưới ở đầu tên các thuộc tính ```protected``` và ```private```
* Các thuộc tính PHẢI được sắp xếp theo cấp độ truy cập: ```public``` &rarr; ```protected``` &rarr; ```private```

```php
<?php
namespace Zend\Config;

class Factory
{
    public static $readers = null;
    public static $writers = null;
    protected static $extensions = array(
        'ini'  => 'ini',
        'json' => 'json',
        'xml'  => 'xml',
        'yaml' => 'yaml',
    );
    ...
}
```

### Phương thức

* Tất cả phương thức PHẢI được khai báo cấp độ truy cập (```public```, ```protected```, và ```private```)
* Tên phương thức PHẢI được khai báo theo định dạng ```camelCase()```
* KHÔNG NÊN thêm dấu gạch dưới ở đầu tên các phương thức ```protected``` và ```private```
* Dấu ngoặc nhọn mở và đóng PHẢI được đặt trên dòng mới lần lượt sau tên và nội dung của phương thức. KHÔNG ĐƯỢC có các khoảng trắng sau các dấu ngoặc nhọn này

```php
<?php
namespace Zend\Debug;

class Debug
{
    public static function dump($var, $label = null, $echo = true)
    {
    }
}
```

* PHẢI __sử dụng động từ__ để đặt tên phương thức
* Các phương thức thiết lập và trả về giá trị thuộc tính của lớp (còn được gọi là các hàm ```getter```, ```setter```)
NÊN được bắt đầu bằng ```get``` và ```set```.

Trong một số trường hợp, ví dụ như muốn gán hoặc lấy giá trị thuộc tính động, bạn CÓ THỂ sử dụng các hàm ```__get``` và ```__set```:

```php
<?php
namespace Zend\View\Model;

class ViewModel implements ModelInterface, ClearableModelInterface
{
    public function __set($name, $value)
    {
    }

    public function __get($name)
    {
    }

    public function setTemplate($template)
    {
    }

    public function getTemplate()
    {
    }
}
```

* Phương thức trả về giá trị ```bool``` NÊN được bắt đầu bằng ```is```, ```has```

```php
<?php
namespace Zend\View\Helper;

class Doctype extends AbstractHelper
{
    public function isXhtml()
    {
    }

    public function isHtml5()
    {
    }
}
```

```php
<?php
namespace Zend\View\Helper;

class ViewModel extends AbstractHelper
{
    public function hasRoot()
    {
    }
}
```

### Tham số phương thức

* Các tham số của phương thức được phân cách nhau bởi dấu phẩy. KHÔNG ĐƯỢC có khoảng trắng nào trước mỗi dấu phẩy, và
PHẢI có duy nhất một khoẳng trắng sau mỗi dấu phẩy.
* Các tham số với giá trị mặc định PHẢI được đặt ở cuối cùng.

```php
<?php
namespace Zend\Json;

class Json
{
    public static function encode($valueToEncode, $cycleCheck = false, $options = array())
    {
    }
}
```

* Các tham số CÓ THỂ được đặt trên nhiều dòng khác nhau, mỗi dòng con PHẢI được thụt vào 4 khoảng trắng.
Trong trường hợp này, tham số đầu tiên PHẢI được đặt trên dòng kế tiếp tên phương thức, và mỗi một tham số PHẢI được đặt
trên một dòng.

```php
<?php
namespace Zend\I18n\Translator;

class Translator
{
    ...
    public function translatePlural(
        $singular,
        $plural,
        $number,
        $textDomain = 'default',
        $locale = null
    ) {
        ...
    }
    ...
}
```

### Lời gọi hàm, phương thức

* Khi thực hiện lời gọi hàm hoặc phương thức, KHÔNG ĐƯỢC có khoảng trắng giữa tên phương thức và dấu mở ngoặc, KHÔNG ĐƯỢC
có khoảng trắng sau dấu mở ngoặc, KHÔNG ĐƯỢC có khoảng trắng trước dấu đóng ngoặc.

* Trong danh sách các tham số, KHÔNG ĐƯỢC có khoảng trắng trước mỗi dấu phẩy và PHẢI có duy nhất một khoảng trắng sau
mỗi dấu phẩy.

```php
<?php
$handle = fopen('http://framework.zend.com/api/zf-version?v=2', 'r');
$module = $matches->getParam(self::MODULE_NAMESPACE, false);
```

* Các tham số CÓ THỂ được tách thành nhiều dòng. Sử dụng một trong hai định dạng sau:

```php
<?php
// Tham số đầu tiên đặt cùng dòng với tên phương thức
//
// Mỗi tham số tiếp theo PHẢI được đặt trên một dòng.
// Các dòng được thẳng hàng với dấu ngoặc mở ngay sau tên phương thức.

// Trích từ Zend\Crypt\BlockCipher::encrypt()
$hash = Pbkdf2::calc(self::KEY_DERIV_HMAC,
                     $this->getKey(),
                     $this->getSalt(),
                     $this->keyIteration,
                     $keySize * 2);
```

```php
<?php
// Tham số đầu tiên đặt khác dòng với tên phương thức
//
// Mỗi tham số tiếp theo PHẢI được đặt trên một dòng.
// Các dòng được thụt vào 4 khoẳng trắng.
// Dấu ngoặc đóng của lời gọi PHẢI được đặt trên một dòng riêng biệt.

// Trích từ Zend\I18n\Translator\Translator::getTranslatedMessage()
$results = $this->getEventManager()->trigger(
    self::EVENT_MISSING_TRANSLATION,
    $this,
    array(
        'message'     => $message,
        'locale'      => $locale,
        'text_domain' => $textDomain,
    ),
    function ($r) {
        return is_string($r);
    }
);
```

* Khi thực hiện liên tiếp các lời gọi phương thức từ một đối tượng (còn được gọi là _method chaining_), lời gọi đầu tiên
PHẢI được đặt trên dòng đầu tiên cùng với tên đối tượng, các lời gọi tiếp theo PHẢI được đặt trên các dòng khác nhau và
được thụt vào thẳng hàng với ```->``` của lời gọi đầu tiên.

```php
<?php
// Trích từ Zend\Mvc\Application::bootstrap()
$event->setApplication($this)
      ->setRequest($this->getRequest())
      ->setResponse($this->getResponse())
      ->setRouter($serviceManager->get('Router'));
```

## Cấu trúc điều khiển

Các cấu trúc điều khiển PHẢI tuân theo các quy định sau:

* KHÔNG ĐƯỢC có khoảng trắng trước dấu ngoặc nhọn mở
* KHÔNG ĐƯỢC có khoảng trắng sau dấu ngoặc nhọn đóng
* PHẢI có một khoảng trắng nằm giữa dấu ngoặc đóng và dấu ngoặc nhọn mở
* Dấu ngoặc nhọn đóng PHẢI được đặt trên dòng kế tiếp sau thân của cấu trúc điều khiển
* Thân của cấu trúc điều khiển PHẢI được thụt vào 4 khoảng trắng
* Thân của mỗi cấu trúc PHẢI được đặt trong cặp dấu ngoặc nhọn

### ```if```, ```elseif```, ```else```

* ```else``` và ```elseif``` PHẢI được đặt trên cùng dòng với dấu ngoặc nhọn đóng của thân cấu trúc trước đó
* NÊN sử dụng từ khoá ```elseif``` thay vì ```else if```. Và vì thế tất cả các từ khoá điều khiển sẽ chỉ có 1 từ.

```php
<?php
if ($expr1) {
    ...
} elseif ($expr2) {
    ...
} else {
    ...
}
```

* Nếu biểu thức điều kiện bao gồm nhiều điều kiện con, bạn CÓ THỂ tách các điều kiện thành nhiều dòng khác nhau.
Trong trường hợp này, mỗi điều kiện PHẢI được đặt trên một dòng. Các dòng thứ 2, 3, ... phải được thụt vào 4 khoảng trắng
so với từ khoá ```if```.

Dấu ngoặc đóng của cấu trúc ```if``` và dấu ngoặc nhọn mở của thân cấu trúc PHẢI được đặt trên một dòng riêng.

```php
<?php
// Trích từ Zend\View\Helper\ServerUrl::setHost()
if (($scheme == 'http' && (null === $port || $port == 80))
    || ($scheme == 'https' && (null === $port || $port == 443))
) {
    ...
}
```

* NÊN sử dụng toán tử ```?:``` nếu có thể.
Ví dụ như cấu trúc ```if else``` sau đây:

```php
<?php
if (is_object($plugin)) {
    $type = get_class($plugin);
} else {
    $type = gettype($plugin);
}
```

có thể được rút gọn lại như sau:

```php
<?php
// Trích từ Zend\ServiceManager\AbstractPluginManager
// \WriterPluginManager::validatePlugin()
$type = is_object($plugin) ? get_class($plugin) : gettype($plugin);
```

Nếu dòng mã quá dài, bạn CÓ THỂ tách các câu lệnh sau ```?``` và ```:``` trên 2 dòng mới, mỗi dòng PHẢI thụt vào thẳng
hàng với ```=``` của dòng đầu tiên.

```php
<?php
// Trích từ Zend\View\Helper\HeadMeta::itemToString()
$tpl = ($this->view->plugin('doctype')->isXhtml())
     ? '<meta %s="%s"/>'
     : '<meta %s="%s">';
```

NÊN sử dụng kiểu rút gọn ```?:``` nếu có thể:

```php
<?php
// Trích từ Zend\Json\Server\Client::__construct
$this->httpClient = $httpClient ?: new HttpClient();
```

* NÊN sử dụng ```switch (true)``` cho đoạn mã có quá nhiều câu lệnh ```if```.

```php
<?php
// Trích từ Zend\Uri\Uri::removePathDotSegments()
switch (true) {
    case ($path == '/.'):
        ...
        break;
    case ($path == '/..'):
        ...
        break;
    case (substr($path, 0, 4) == '/../'):
        ...
        break;
    case (substr($path, 0, 3) == '/./'):
        ...
        break;
    case (substr($path, 0, 2) == './'):
        ...
        break;
    case (substr($path, 0, 3) == '../'):
        ...
        break;
    default:
        ...
        break;
}
```

### ```switch```, ```case```

* Câu lệnh ```case``` PHẢI được thụt vào 4 khoảng trắng so với ```switch```
* Từ khoá ```break``` (hoặc bất kỳ từ khoá kết thúc cấu trúc, ví dụ ```return```) PHẢI được thụt vào giống như thân của ```case```
* PHẢI có một _comment_ như ```// no break``` nếu _case_ tương ứng không rỗng và cấu trúc _switch_ không kết thúc tại _case_ này

```php
<?php
switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
```

### ```while```, ```do while```

```php
<?php
while ($expr) {
    ...
}
```

```php
<?php
do {
    ...
} while ($expr);
```

### ```for```

```php
<?php
for ($i = 0; $i < 10; $i++) {
    ...
}
```

### ```foreach```

```php
<?php
foreach ($array as $key => $value) {
    ...
}
```

### ```try```, ```catch```

```php
<?php
try {
    ...
} catch (FirstExceptionType $e) {
    ...
} catch (OtherExceptionType $e) {
    ...
}
```

## Tham khảo

* [Why would one omit the close tag?](http://stackoverflow.com/questions/4410704/why-would-one-omit-the-close-tag)
* [Khái niệm _Method chaining_](http://en.wikipedia.org/wiki/Method_chaining)