---
layout: post
title:  "Quy tắc đánh số phiên bản phần mềm [Vietnamese]"
date:   2013-10-11 00:40:15
---

Tài liệu này hướng dẫn bạn một số quy tắc đánh số phiên bản phần mềm dựa trên chuẩn [SemVer](http://semver.org) (viết tắt của Semantic Versioning),
được nhiều phần mềm hiện nay sử dụng.

Ngày nay, các phần mềm sử dụng nhiều thư viện bên ngoài để giảm thời gian phát triển. Việc cài đặt một phần mềm có thể
phải kéo theo việc tải về và cài đặt các thư viện kèm theo. Đồng thời bạn phải kiểm tra sự tương thích giữa phần mềm và
các thư viện đó.

Để giảm thiểu thời gian và rủi ro của việc này, một số phần mềm cho phép bạn đăng ký phần mềm của bạn trong đó xác định
tên cùng phiên bản của các thư viện cần thiết. Việc đánh số phiên bản phần mềm vì thế rất quan trọng và tất cả các phần
mềm trong hệ thống cần tuân theo một quy chuẩn.

Có thể kể ra một số hệ thống quản lý gói phần mềm như:

* [Bower](http://bower.io), phát triển bởi Twitter, quản lý các gói cho _front end_
* [npm](https://npmjs.org), quản lý các thư viện JS
* [Packagist](https://packagist.org), quản lý các thư viện PHP

Hai ví dụ dưới đây cho bạn hình dung về việc khai báo thông tin của một thư viện kèm theo phiên bản của các thư viện cần thiết:

- [Twitter Bootstrap](https://github.com/twitter/bootstrap)

File [component.json](https://github.com/twitter/bootstrap/blob/master/component.json)

```
{
    "name": "bootstrap",
    "version": "2.3.2",
    "dependencies": {
        "jquery": "~1.8.0"
    }
}
```

- [LessCSS](http://lesscss.org)

File [package.json](https://github.com/cloudhead/less.js/blob/master/package.json)

```
{
    "name": "less",
    "version": "1.4.0-b4",
    "description": "Leaner CSS",
    "homepage": "http://lesscss.org",
    ...
    "repository": {
        "type": "git",
        "url": "https://github.com/cloudhead/less.js.git"
    },
    ...
    "optionalDependencies": {
        "mime": "1.2.x",
        "request": ">=2.12.0",
        "mkdirp": "~0.3.4",
        "ycssmin": ">=1.0.1"
    },
    "devDependencies": {
        "diff": "~1.0"
    },
    ...
}
```

## Các quy tắc đánh số

SemVer đưa ra các tiêu chuẩn sau cho việc đánh số:

1) Phần mềm sử dụng SemVer PHẢI khai báo các API công cộng. Các API này có thể được khai báo trong mã nguồn chính hoặc trong tài liệu văn bản kèm theo.
Tuy nhiên, khi hoàn thành, việc khai báo API phải được thực hiện chính xác và hoàn chỉnh.

2) Mỗi phiên bản đánh số thông thường PHẢI theo chuẩn X.Y.Z, trong đó X, Y, Z là các số nguyên không âm đại diện cho:

* X là phiên bản chính (_Major version_)
* Y là phiên bản phụ (_Minor version_)
* Z là phiên bản vá (_Path version_)

Khi phát hành một phiên bản mới, X, Y, Z PHẢI được tăng ổn định và có thứ tự. Ví dụ 1.9.0 &rarr; 1.10.0 &rarr; 1.10.1

3) Một khi phiên bản mới đã được phát hành, tất cả nội dung (mã nguồn, API) của phiên bản đó KHÔNG ĐƯỢC thay đổi.

Bất kỳ thay đổi phát sinh nào đều PHẢI được công bố như phát hành một phiên bản mới.

4) Tất cả các phiên bản phát triển ban đầu NÊN được đánh số dạng ```0.y.z```. Bạn có thể thực hiện bất kỳ thay đổi nào trong các phiên bản ở giai đoạn này.
Các hàm API công cộng KHÔNG NÊN được coi là ổn định.

5) Phiên bản 1.0.0 bắt đầu cung cấp các hàm API công cộng. Kể từ phiên bản này, việc tăng số phiên bản phụ thuộc vào cách thay đổi các API.

6) PHẢI tăng phiên bản vá Z (```x.y.Z```, với ```x > 0```) nếu phiên bản này:

* Chỉ sửa các lỗi phát sinh, và đảm bảo tương thích với các bản cũ trước đó.

7) PHẢI tăng phiên bản phụ Y (```x.Y.z```, với ```x > 0```) nếu phiên bản này:

* Tương thích ngược với các bản cũ có cùng phiên bản chính
* Cung cấp mới một API công cộng
* Có API công cộng được khuyến cáo là không nên dùng (_deprecated_)

Bạn CÓ THỂ tăng ```Y``` nếu:

* Thêm mới một chức năng quan trọng
* Có thêm bất kỳ cải thiện trong mã nguồn

Phiên bản phụ CÓ THỂ bao gồm những thay đổi ở cấp độ vá.
Phiên bản vá ```z``` PHẢI được thiết lập về 0 khi tăng số phiên bản phụ ```y```.

8) PHẢI tăng phiên bản chính X (```X.y.z```, với ```X > 0```) nếu phiên bản này:

* Có thêm API công cộng không tương thích với các phiên bản cũ

Phiên bản chính CÓ THỂ bao gồm những thay đổi ở cấp độ vá và cấp độ phụ.
Phiên bản phụ ```y``` và vá ```z``` PHẢI được thiết lập về 0 khi tăng số phiên bản chính ```x```.

9) Mỗi phiên bản CÓ THỂ sử dụng dấu gạch ngang (-), và một chuỗi dấu chấm (.) để tách các định danh cho một phiên bản tiền phát hành hoặc một phiên bản vá.
Các tên hợp lệ PHẢI bao gồm các chữ cái thường, chữ cái hoa, chữ số và dấu gạch ngang (```0-9A-Za-z-```).
Phiên bản tiền phát hành có quyền ưu tiên thấp hơn phiên bản bình thường có liên quan.

Ví dụ: ```1.0.0-alpha```, ```1.0.0-alpha.1```

10) Các bản _metadata_ CÓ THỂ được biểu thị bằng cách thêm một dấu cộng (+) và một loạt dấu chấm (.) tách các định danh
ngay sau số hiệu phiên bản tiền phát hành hoặc phiên bản vá.

Các định danh sau dấu cộng PHẢI bao gồm một chuỗi chữ cái hoa, thường, chữ số, hoặc ký tự gạch ngang (-).

Phiên bản metadata NÊN được bỏ qua khi xác định quyền ưu tiên giữa các phiên bản. Do đó 2 phiên bản chứa các phiên bản chính/phụ/vá giống nhau,
nhưng số hiệu _metadata_ khác nhau được coi là tương đồng.

Ví dụ: ```1.0.0-alpha+001```, ```1.0.0+20130313144700```, ```1.0.0-beta+exp.sha.5114f85```

11) Trước khi phát hành, quyền ưu tiên PHẢI được tính toán dựa trên thứ tự số hiệu của phiên bản chính, phụ, vá, và tiền phát hành (bản _metadata_ không có quyền ưu tiên).
Với số hiệu của mỗi phiên bản, bao gồm các chữ số (được so sánh như so sánh các số tự nhiên), ký tự (-) và các định danh (đươc so sánh theo thứ tự sắp xếp ASCII).
Định số luôn có ưu tiên thấp hơn các định dạng khác.

Ví dụ: ```1.0.0-alpha < 1.0.0-alpha.2 < 1.0.0-beta.2 < 1.0.0-beta.11 < 1.0.0-rc.1 < 1.0.0```

## Tham khảo

* [Semantic Versioning](http://semver.org)
* [Software versioning](http://en.wikipedia.org/wiki/Software_versioning)