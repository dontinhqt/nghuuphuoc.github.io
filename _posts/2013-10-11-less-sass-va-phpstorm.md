---
layout: post
title:  "LESS, SASS và PHP Storm [Vietnamese]"
date:   2013-10-11 00:00:40
---

Tài liệu này hướng dẫn cách cài đặt trình biên dịch cho LESS, SASS và tích hợp với PHP Storm.

## Cài đặt trên Mac OS

### Cài bộ biên dịch LESS

Bộ biên dịch LESS được cài thông qua _npm_.

npm là viết tắt của _Node Packaged Modules_. Ban đầu, đây là nơi lưu trữ các gói, thư viện cho [NodeJs](http://nodejs.org).
Về sau, tác giả của nhiều thư viện khác cũng chọn đây là nơi lưu trữ, cập nhật phần mềm, thư viện của họ.
Do đó ngoài các thư viện cho NodeJs, bạn có thể tìm thấy cả các thư viện chạy độc lập (ví dụ như _less_). Danh sách các
gói được liệt kê trên website <https://npmjs.org>

Các giai đoạn cài đặt bao gồm: XCode &rarr; Phần mềm quản lý các gói cho Mac OS &rarr; NodeJs &rarr; npm &rarr; LESS

#### Cài đặt XCode

* Tải về phiên bản mới nhất của [XCode](https://developer.apple.com/xcode)
* Mở XCode từ thư mục _Applications_. Chọn menu _XCode &rarr; Preferences_. Chọn tab _Downloads_, nhấn nút _Install_ ứng với dòng _Command Line Tools_.

![Cài đặt _Command Line Tools_](/img/xcode-command-tool.png)

* Đồng ý với bản quyền của XCode bằng cách chạy dòng lệnh sau:

```bash
$ sudo xcodebuild -license
```

Nhấn phím _space_ cho đến khi hiển thị hết nội dung bản quyền. Gõ _agree_ và nhấn _Enter_.

#### Cài đặt trình quản lý gói cho Mac OS (MacPort)

Bạn có thể bỏ qua bước này nếu MacPort đã được cài đặt. Có thể kiểm tra MacPort đã được cài đặt hay chưa bằng lệnh

```bash
$ which port
```

Có 2 cách để cài đặt một gói, thư viện trên Mac OS:

* Biên dịch

Hầu hết các thư viện được viết dựa trên ngôn ngữ C, C++, vì thế cách thông thường để cài đặt các thư viện này là sử dụng
bộ diên dịch. Nếu thư viện đó phụ thuộc vào các thư viện khác, bạn phải tải về và cài đặt các thư viện phụ thuộc trước.

Rõ ràng việc này hết sức mất thời gian và không kém phần rủi ro nếu quá trình cài đặt thư viện phụ thuộc bị lỗi hoặc
không đảm bảo tính tương thích với thư viện ban đầu.

* Sử dụng phần mềm quản lý gói

Phần mềm quản lý gói cho phép bạn tải về và cài đặt các gói (kèm các gói phụ thuộc) một cách tự động và dễ dàng. Bạn
cũng có thể đã biết các phần mềm tương tự như _yum_ (trên Centos), _apt_ (trên Ubuntu).

Với Mac OS, phổ biến 3 phần mềm dạng này là [Mac Port](http://www.macports.org), [Homebrew](http://mxcl.github.io/homebrew)
và [Fink](http://www.finkproject.org).
Tuy nhiên bạn nên sử dụng Mac Port vì nó cung cấp rất nhiều gói.

Thực hiện các bước sau để cài MacPort:

* Tải về bản mới nhất của [MacPort](http://www.macports.org/install.php).
Tại thời điểm viết tài liệu này, phiên bản mới nhất của MacPort dành cho _Mac OS Mountain Lion_ là [2.1.3](https://distfiles.macports.org/MacPorts/MacPorts-2.1.3-10.8-MountainLion.pkg)

* Click đúp chuột vào file tải về và thực hiện theo hướng dẫn cài đặt.

![Install MacPort](/img/install-macport.png)

* Để sử dụng MacPort, bạn phải thêm đường dẫn đến file ```port``` vào biến môi trường ```$PATH``` bằng cách chạy dòng lệnh sau:

```bash
$ export PATH=/opt/local/bin:/opt/local/sbin:$PATH
```

Để cài đặt gói thư viện nào đó, sử dụng câu lệnh sau (```PackageName``` là tên gói):

```bash
$ sudo port install PackageName
```

Cú pháp câu lệnh này hoàn toàn tương tự việc cài đặt gói sử dụng ```yum``` và ```apt```:

```bash
$ yum install PackageName
$ apt-get install PackageName
```

#### Cài đặt NodeJs

Bạn có thể bỏ qua bước này nếu NodeJs đã được cài đặt. Có thể kiểm tra NodeJs đã được cài đặt hay chưa bằng lệnh:

```bash
$ which node
```

Chạy dòng lệnh sau:

```bash
$ sudo port install nodejs
```

Kiểm tra đường dẫn và phiên bản của NodeJs:

```bash
$ which node
/opt/local/bin/node
$ node -v
v0.10.6
```

#### Cài đặt npm

Bạn có thể bỏ qua bước này nếu ```npm``` đã được cài đặt. Có thể kiểm tra npm đã được cài đặt hay chưa bằng lệnh

```bash
$ which npm
```

Chạy dòng lệnh sau:

```bash
$ sudo port install npm
```

Kiểm tra đường dẫn và phiên bản của npm:

```bash
$ which npm
/opt/local/bin/npm
$ npm -v
1.2.21
```

#### Cài đặt LESS

npm cho phép bạn cài đặt bất kỳ gói nào từ <https://npmjs.org> với cú pháp ```npm install PackageName```.
Thư viện mà bạn muốn cài đặt (cùng với các thư viện phụ thuộc) sẽ được tự động tải về và lưu trữ tại thư mục có tên là
```node_modules```, cùng thư mục bạn chạy dòng lệnh trên.

Nếu bạn muốn cài thư viện ở cấp độ _global_, sử dụng tham số _-g_:

```bash
$ sudo npm install -g PackageName
```

Để cài đặt less, bạn chạy dòng lệnh sau:

```bash
$ sudo npm install -g less
```

Bạn có thể kiểm tra đường dẫn cũng như phiên bản của _less_ bằng các lệnh:

```bash
$ which less
/usr/bin/less
$ less -V
less 418
Copyright (C) 1984-2007 Mark Nudelman
```

### Cài bộ biên dịch SASS

Chạy dòng lệnh sau:

```bash
$ sudo gem install sass
```

SASS sẽ được cài đặt ở thư mục _/usr/bin_. Bạn có thể xác định đường dẫn của bộ biên dịch SASS, SCSS bằng lệnh ```which```:

```bash
$ which sass
/usr/bin/sass
$ which scss
/usr/bin/scss
$ sass -v
Sass 3.2.9 (Media Mark)
```

## Cài đặt trên Windows

### Cài bộ biên dịch LESS

Để cài LESS, trước hết bạn phải cài đặt _NodeJs_ và _npm_. Xem thông tin về npm ở mục _Cài đặt LESS trên Mac OS_.

#### Cài đặt NodeJs và npm

* Tải về phiên bản mới nhất của [NodeJs dành cho Windows](http://nodejs.org/download/).
Tại thời điểm viết tài liệu này, NodeJs cung cấp bản [0.10.9](http://nodejs.org/dist/v0.10.9/node-v0.10.9-x86.msi) cho Windows.

* Nhấp đúp chuột vào file tải về và thực hiện theo hướng dẫn.

![Cài đặt NodeJS trên Windows](/img/nodejs-win.png)

* Kiểm tra phiên bản cài đặt của NodeJs và npm bằng cách chạy các dòng lệnh sau từ cửa sổ _Command Line_:

```bash
C:\> node -v
v0.10.3
C:\> npm -v
1.2.17
```

Có thể mở cửa sổ _Command Line_ bằng một trong hai cách sau:

- Nhấn _Start &rarr; Run_, gõ ```cmd``` và nhấn _Enter_
- hoặc nhấn _Start &rarr; All Programs &rarr; Accessories_, sau đó chọn _Command Prompt_

#### Cài đặt LESS

![Cài đặt LESS trên Windows](/img/npm-less-win.png)

Từ cửa sổ _Command Line_, chạy dòng lệnh sau:

```bash
C:\> npm install -g less
```

npm sẽ tự động tải về và cài đặt less cùng các thư viện phụ thuộc. Thông thường, _lessc_ (tên của bộ biên dịch less)
được lưu tại đường dẫn

```bash
C:\Users\{Tên Người Dùng}\AppData\Roaming\npm\lessc.cmd
```

trong đó ```{Tên Người Dùng}``` là tên tài khoản Windows của người dùng thực hiện việc cài đặt.

Bạn có thể kiểm tra phiên bản của bộ biên dịch less bằng lệnh:

```bash
C:\Users\{Tên Người Dùng}\AppData\Roaming\npm> less.cmd -v
lessc 1.3.3 (LESS Compiler) [JavaScript]
```

### Cài bộ biên dịch SASS

Bộ diên dịch SASS được cài đặt thông qua _gem_ được phát triển trên ngôn ngữ Ruby, vốn không được Windows cung cấp sẵn
như các hệ điều hành khác, do đó trước hết bạn phải cài Ruby.

#### Cài đặt Ruby

* Tải về phiên bản Ruby cho Windows tại [RubyInstaller](http://rubyinstaller.org/downloads).
Tại thời điểm viết tài liệu, RubyInstaller cung cấp phiên bản 2.0.0 cho Windows, gồm [bản 32 bit](http://rubyforge.org/frs/download.php/76955/rubyinstaller-2.0.0-p195.exe)
và [bản 64 bit](http://rubyforge.org/frs/download.php/76956/rubyinstaller-2.0.0-p195-x64.exe).

* Nhấp đúp file tải về và thực hiện theo các hướng dẫn cài đặt.

Lưu ý chọn _Add Ruby executables to your PATH_ tại hộp thoại xác định đường dẫn thư mục sẽ cài Ruby (mặc định là
```C:\Ruby200-x64```, có thể thay đổi tuỳ phiên bản Ruby và hệ điều hành).

![Cài đặt Ruby trên Windows](/img/ruby-win.png)

#### Cài đặt SASS

Chạy dòng lệnh sau:

```bash
C:\> gem install sass
Fetching: sass-3.2.9.gem (100%)
Successfully installed sass-3.2.9
Parsing documentation for sass-3.2.9
Installing ri documentation for sass-3.2.9
```

gem tự động tải về và cài đặt sass cùng với các thư viện phụ thuộc. Bộ biên dịch sass được lưu tại đường dẫn ```{Thư mục cài Ruby}/bin/sass```.
Bạn có thể kiểm tra phiên bản của bộ biên dịch này bằng lệnh:

```bash
C:\> sass -v
Sass 3.2.9 (Media Mark)
```

## Tích hợp với PHP Storm

PHP Storm cung cấp một số bộ _File Watchers_ có nhiệm vụ theo dõi sự thay đổi của _file_. Nếu nội dung các _file_ bị
thay đổi, PHP Storm sẽ tự động thực hiện một số thao tác tương ứng nào đó.
Ví dụ _SCSS watcher_ sẽ tự động biên dịch file ```.scss``` ra file ```.css``` ngay khi file ```.scss``` ban đầu được cập nhật.

PHP Storm cũng yêu cầu bạn cấu hình _file watcher_ nếu file bạn mở có phần mở rộng được hỗ trợ bởi các _watcher_ có sẵn.
Hình sau đây minh hoạ việc PHP Storm yêu cầu thiết lập _SCSS watcher_ khi mở một file SASS (có phần mở rộng là ```.scss```):

![Cấu hình SCSS watcher](/img/phpstorm-scss-watcher.png)

Để thiết lập _SCSS watcher_, bạn có thể nhấn vào _Add watcher_ từ hộp thoại thông báo như hình trên.
Hoặc có thể thực hiện theo các bước sau đây:

* Vào menu _PHP Storm &rarr; Preferences_. Trên Windows, bạn vào menu _File &rarr; Settings_
* Chọn _File Watcher_ từ cột bên trái
* Ở hộp thoại bên phải, nhấn nút _+_ từ thanh công cụ bên dưới cùng của hộp thoại, chọn _SCSS_ từ menu thả xuống.

![Thiết lập SCSS watcher](/img/phpstorm-add-scss.png)

* Trong ô _Program_, nhập vào đường dẫn của bộ biên dịch SCSS. Nhấn nút _OK_.
Bảng sau chỉ ra đường dẫn đến các bộ biên dịch LESS, SASS sau khi thực hiện các bước cài đặt trong tài liệu:

| Bộ biên dịch | Đường dẫn
|--------------|----------
| LESS         | Trên Mac OS: ```/usr/bin/less```
|              | Trên Windows: ```C:\Users\{Tên Người Dùng}\AppData\Roaming\npm\lessc.cmd```
| SASS         | Trên Mac OS: ```/usr/bin/sass```
|              | Trên Windows: ```{Thư mục cài Ruby}\bin\sass```

Thực hiện các thao tác tương tự để thiết lập _LESS watcher_.