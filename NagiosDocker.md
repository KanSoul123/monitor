# Setup Nagios Docker on Ubuntu
1. Cài Ubuntu ( tùy vào ubuntu mà cài)
3. Cài Docker
     ## Cập nhật hệ thống
     `sudo apt update`
     ## Cài các gói cần thiết
     `sudo apt install apt-transport-https ca-certificates curl software-properties-common`
     ## Khóa GPG
     `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`
     ## Thêm kho dữ Docker vào nguồn APT
     `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" `
     ## Cập nhật cơ sở dữ liệu với các gói Docker từ repo mới được thêm vào
     `apt-cache policy docker-ce`
     #### Output
     ![image](https://github.com/user-attachments/assets/218a0a1a-a950-4753-b9e6-55f810c00216)
     ## Cài đặt Docker
     `sudo apt install docker-ce`
     ## Kiểm tra
     `sudo systemctl status docker `
     #### Output
   ![image](https://github.com/user-attachments/assets/3a8a1555-33f4-467f-ab50-78d79c462eb6)
5. Cài Nagios Images
     ## Tải images về máy và kiểm tra
     `docker pull jasonrivers/nagios`
   
     `docker images`
   
     ![image](https://github.com/user-attachments/assets/44dd05f7-de0f-4ddc-8ba4-4ff3b617df47)
   
     ## Tạo thư mục
     #### Tạo thư mục nagios trong đường dẫn /usr/local/etc
      `/usr/local/etc# mkdir nagios`
     #### Trong thư mục nagios tạo 5 thư mục khác (var, etc, graph-var, graph-etc, custom-plugins)
     ![image](https://github.com/user-attachments/assets/29425360-c6a1-4b22-8ff6-03cbb945229e)
     #### Phân quyền cho thư mục var và etc
     > phân quyền vì sẽ xuất hiện lỗi Permission denied
      ```bash
      chmod -R 777 /usr/local/etc/nagios/etc
      chmod -R 777 /usr/local/etc/nagios/var
      ```
      ## Chạy Docker
     ```bash
     docker run --name nagios \
       -v /usr/local/etc/nagios/etc:/opt/nagios/etc/ \
       -v /usr/local/etc/nagios/var:/opt/nagios/var/ \
       -v /usr/local/etc/nagios/custom-plugins:/opt/Custom-Nagios-Plugins \
       -v /usr/local/etc/nagios/graph-var:/opt/nagiosgraph/var \
       -v /usr/local/etc/nagioss/graph-etc:/opt/nagiosgraph/etc \
       -dp 8080:80 jasonrivers/nagios:latest
     ```
     Khi bạn sử dụng tham số -v trong Docker, bạn đang mount một thư mục từ máy chủ (host) vào một thư mục trong container. Tuy nhiên, có một số điểm cần lưu ý:
     Cấu trúc thư mục trong container: Đường dẫn mà bạn mount vào trong container phải tương ứng với cấu trúc thư mục trong container.
     Trong trường hợp của Nagios, /opt/nagios/etc/ là thư mục cấu hình trong container, và /opt/nagios/etc/objects/ là thư mục con chứa các file cấu hình cụ thể (như windows.cfg, localhost.cfg).
     >Lưu ý: Trong câu lệnh của bạn, khi sử dụng dấu \ để ngắt dòng, hãy chắc chắn rằng không có dấu cách sau dấu \, vì điều này có thể gây lỗi. Hãy viết lại lệnh đúng cú pháp.
     
     Truy cập web bằng <ip_của_nagios>:8080
   Tài khoản mặc định là: nagiosadmin/nagios
   
7. Giám sát Windows Server
     #### Cấu hình trong /usr/local/etc/nagios/etc/objects file windows.cfg
     ![image](https://github.com/user-attachments/assets/b163bbb4-1eef-4b73-997d-50a832dec3a5)
   
      chỉnh Ip về của máy windows server cần giám sát rồi lưu và thoát
     #### Thêm đường dẫn tới file windows.cfg nằm ở file nagios.cfg trong thư mục (có sẵn chỉ cần bỏ #)
     ![image](https://github.com/user-attachments/assets/ebc4c4ab-8567-43f2-b866-03b2c4cd34be)
   
      Khởi động lại
     `docker restart <container_id>`
     #### Cài đặt NSClient++ trên windows server
      ![image](https://github.com/user-attachments/assets/b3b608ed-14f8-4074-b7b9-56999cc026ca)
   
      Chỉnh Allow hosts: Là Ip của máy Nagios
8. Kết quả
     
     ![image](https://github.com/user-attachments/assets/0359423f-0372-40e5-915e-3bea61275970)

