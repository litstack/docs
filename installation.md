# Cài đặt

## Yêu cầu

Cần có một dự án Laravel hiện tại để cài đặt gói này cũng như kết nối cơ sở dữ liệu. Gói yêu cầu **PHP 7.4+** và **Laravel 7+**. Như cũng như tất cả [các yêu cầu](https://docs.spatie.be/laravel-medialibrary/v8/requirements) của **spatie/laravel-medialibrary 8.2+**.

## Thiết lập

Cài đặt gói vào một ứng dụng Laravel hiện có thông qua Composer:

```shell
composer require litstack/litstack
```

Ứng dụng sẽ tự động đăng ký các nhà cung cấp dịch vụ cần thiết. Các bước tiếp theo là xử lý tất cả các lần xuất bản và di chuyển bằng cách nhập nội dung sau lệnh artisan:

```shell
php artisan lit:install
```

Bây giờ tất cả các mô hình đã được chuyển đến thư mục `app/models`, tất cả các tệp bắt buộc đã được xuất bản và tất cả quá trình di chuyển đã được thực hiện.

Có thể truy cập giao diện quản trị thông qua đường dẫn `/admin`. Route có thể được thay đổi trong tệp cấu hình `lit.php` bằng cách thay đổi khóa `route_prefix`.

Bước cuối cùng là tạo người dùng quản trị để bạn có thể đăng nhập vào phần phụ trợ:

```shell
php artisan lit:admin
```

Trình hướng dẫn sẽ hướng dẫn bạn quy trình nhập người dùng cần thiết dữ liệu.
