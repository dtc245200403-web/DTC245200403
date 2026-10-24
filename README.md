DTC245200403 – WEBSITE QUẢNG BÁ SẢN PHẨM (WORDPRESS)
1. Giới thiệu dự án
Dự án xây dựng website quảng bá sản phẩm bằng WordPress, triển khai bằng Docker Compose. Hệ thống sử dụng MySQL để lưu trữ dữ liệu, Nginx làm Reverse Proxy và các công cụ giám sát để theo dõi hoạt động của hệ thống.
Công nghệ sử dụng
- WordPress: Quản lý nội dung website.
- MySQL: Lưu trữ dữ liệu website.
- phpMyAdmin: Quản lý cơ sở dữ liệu.
- Nginx: Reverse Proxy và HTTPS.
- Prometheus: Thu thập metrics.
- Grafana: Hiển thị dashboard và truy vấn log.
- Loki: Lưu trữ và truy vấn log.
- Promtail: Thu thập log và gửi đến Loki.
- cAdvisor: Theo dõi tài nguyên container Docker.
- Docker Compose: Triển khai và quản lý các dịch vụ.
2. Yêu cầu hệ thống
Trước khi chạy dự án, cần cài đặt:
- Git
- Docker Desktop (Windows) hoặc Docker Engine (Linux)
- Docker Compose
- Trình duyệt web
3. Tải mã nguồn
Mở Terminal và chạy:
git clone https://github.com/dtc245200403-web/DTC245200403.git
cd DTC245200403
4. Cấu hình trước khi chạy
1. Kiểm tra file docker-compose.yml.
2. Kiểm tra các file cấu hình trong thư mục nginx/ và monitoring/.
3. Nếu dự án cần biến môi trường, tạo file .env theo cấu hình do người triển khai cung cấp.
4. Không công khai mật khẩu, token, file .env có bí mật hoặc private key SSL.
Các file cấu hình chính:
- docker-compose.yml: Khai báo các dịch vụ.
- nginx/conf.d/default.conf: Cấu hình Nginx.
- monitoring/prometheus/prometheus.yml: Cấu hình Prometheus.
- monitoring/loki/loki-config.yaml: Cấu hình Loki.
- monitoring/promtail/promtail-config.yml: Cấu hình Promtail.
5. Khởi chạy và kiểm tra hệ thống
Tại thư mục có file docker-compose.yml, chạy:
docker compose up -d
Kiểm tra trạng thái các container:
docker compose ps
Xem log khi có lỗi:
docker compose logs
6. Truy cập website và quản trị WordPress
Website đang được cấu hình để truy cập tại:

https://localhost

Trang quản trị WordPress:

https://localhost/wp-admin

Đăng nhập bằng tài khoản quản trị WordPress đã được thiết lập cho môi trường này.
Đăng nội dung quảng bá sản phẩm
1. Trong WordPress, mở Posts → Add New Post để tạo bài giới thiệu sản phẩm.
2. Nhập tên sản phẩm, mô tả, hình ảnh và thông tin cần thiết.
3. Chọn Publish để đăng bài.
4. Mở trang chủ website để kiểm tra nội dung hiển thị.
Có thể sử dụng Pages để tạo trang Giới thiệu hoặc Liên hệ, và Media để quản lý hình ảnh.
7. Truy cập các dịch vụ khác
Địa chỉ và cổng của từng dịch vụ phụ thuộc vào cấu hình thực tế trong docker-compose.yml và Nginx. Kiểm tra cấu hình trước khi truy cập.
Dịch vụ	Mục đích
phpMyAdmin	Quản lý cơ sở dữ liệu MySQL
Prometheus	Kiểm tra metrics và trạng thái target
Grafana	Xem dashboard và truy vấn log
Loki	Lưu trữ log, thường được truy vấn qua Grafana
cAdvisor	Theo dõi tài nguyên container


8. Hướng dẫn truy vấn log Nginx bằng Grafana và Loki
8.1. Mở Explore
1. Đăng nhập Grafana.
2. Mở Explore.
3. Chọn nguồn dữ liệu Loki.
4. Nhập truy vấn vào ô LogQL.
5. Chọn Run query hoặc nhấn Shift + Enter.
8.2. Truy vấn 1 – Xem log của Nginx Proxy
{container="nginx_proxy"}
Truy vấn này chọn các log stream có nhãn container bằng nginx_proxy. Kết quả phụ thuộc vào nhãn thực tế do Promtail/Loki ghi nhận.
8.3. Truy vấn 2 – Lọc log có mã HTTP 200
{container="nginx_proxy"} |= " 200 "
Truy vấn này tìm các dòng log có chứa chuỗi 200. Chỉ dùng được nếu log truy cập Nginx có chứa mã trạng thái theo định dạng tương ứng.
8.4. Truy vấn 3 – Tìm log có nội dung error hoặc failed
{container="nginx_proxy"} |~ "(?i)error|failed"
Truy vấn này lọc các dòng log có chứa error hoặc failed, không phân biệt chữ hoa/chữ thường. Kết quả phụ thuộc nội dung log thực tế.
8.5. Kiểm tra kết quả
- Chọn khoảng thời gian phù hợp ở Grafana.
- Chạy từng truy vấn.
- Xác nhận có bản ghi log trả về.
- Chụp ảnh màn hình thể hiện truy vấn và kết quả để đưa vào báo cáo.
Nếu không có kết quả, kiểm tra lại nhãn log bằng Label browser, kiểm tra cấu hình Promtail và xác nhận container có phát sinh log.
9. Giám sát hệ thống
Prometheus
Mở giao diện Prometheus theo cổng đã cấu hình và kiểm tra các target có trạng thái hoạt động hay không.
Grafana
Mở dashboard đã cấu hình để theo dõi các metrics của container, web server và database. Dữ liệu hiển thị phụ thuộc vào các exporter và target đã được cấu hình.
cAdvisor
Dùng giao diện cAdvisor theo cổng được khai báo trong docker-compose.yml để xem thông tin tài nguyên container.
10. Dừng và khởi động lại hệ thống
Dừng các dịch vụ:
docker compose down
Khởi chạy lại:
docker compose up -d
Không xóa volume hoặc dữ liệu database nếu chưa sao lưu dữ liệu cần giữ lại.
11. Lưu ý bảo mật
- Không đưa mật khẩu, token hoặc thông tin nhạy cảm lên repository công khai.
- Không commit file .env nếu có chứa bí mật.
- Không công khai private key như nginx.key.
- Kiểm tra .gitignore trước khi đẩy mã nguồn.
- Chỉ mở các cổng cần thiết và sử dụng mật khẩu mạnh.
- Chứng chỉ SSL tự ký có thể khiến trình duyệt hiển thị cảnh báo trong môi trường thử nghiệm.
12. Repository
Mã nguồn dự án:

https://github.com/dtc245200403-web/DTC245200403
Ghi chú: Cổng truy cập của phpMyAdmin, Prometheus, Grafana và cAdvisor, thông tin đăng nhập, tên dịch vụ và nhãn LogQL phải được xác nhận theo cấu hình thực tế của project. README này không bao gồm mật khẩu.