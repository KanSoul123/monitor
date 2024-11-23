# Cài đặt OpenCTI bằng Docker
## 1. Cài Docker 
  ### Cập nhật hệ thống
  `sudo apt update`
  
  ###Cài các gói cần thiết
  
  `sudo apt install apt-transport-https ca-certificates curl software-properties-common`
  
  ###Khóa GPG
  
  `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`
  
  ### Thêm kho dữ Docker vào nguồn APT
  `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" `
  ### Cập nhật cơ sở dữ liệu với các gói Docker từ repo mới được thêm vào
  `apt-cache policy docker-ce`
  ##### Output
  ![image](https://github.com/user-attachments/assets/218a0a1a-a950-4753-b9e6-55f810c00216)
  ### Cài đặt Docker
  `sudo apt install docker-ce`
  ### Kiểm tra
  `sudo systemctl status docker `
  ##### Output
   ![image](https://github.com/user-attachments/assets/3a8a1555-33f4-467f-ab50-78d79c462eb6)
  ### Cài đặt Docker Compose
  `sudo apt install docker-compose`
## 2. Cài OpenCTI
  ### Tạo thư mục chứa Images OpenCti (mình tạo trong đường dẫn /root/opencti)
  `cd /root/ | mkdir opencti`
  ### Vào thư mục opencti và tải 
  `git clone https://github.com/OpenCTI-Platform/docker.git`
  
  Tải xong thì xuất hiện thư mực docker trong opencti vào thư mục docker
  > pwd : /root/opencti

  `cd docker`
  ### Cấu hình môi trường trong file .env
  ```
  sudo apt install -y jq
  (cat << EOF
  OPENCTI_ADMIN_EMAIL=admin@opencti.io
  OPENCTI_ADMIN_PASSWORD=ChangeMePlease
  OPENCTI_ADMIN_TOKEN=$(cat /proc/sys/kernel/random/uuid)
  OPENCTI_BASE_URL=http://localhost:8080
  OPENCTI_HEALTHCHECK_ACCESS_KEY=$(cat /proc/sys/kernel/random/uuid)
  MINIO_ROOT_USER=$(cat /proc/sys/kernel/random/uuid)
  MINIO_ROOT_PASSWORD=$(cat /proc/sys/kernel/random/uuid)
  RABBITMQ_DEFAULT_USER=guest
  RABBITMQ_DEFAULT_PASS=guest
  ELASTIC_MEMORY_SIZE=4G
  CONNECTOR_HISTORY_ID=$(cat /proc/sys/kernel/random/uuid)
  CONNECTOR_EXPORT_FILE_STIX_ID=$(cat /proc/sys/kernel/random/uuid)
  CONNECTOR_EXPORT_FILE_CSV_ID=$(cat /proc/sys/kernel/random/uuid)
  CONNECTOR_IMPORT_FILE_STIX_ID=$(cat /proc/sys/kernel/random/uuid)
  CONNECTOR_EXPORT_FILE_TXT_ID=$(cat /proc/sys/kernel/random/uuid)
  CONNECTOR_IMPORT_DOCUMENT_ID=$(cat /proc/sys/kernel/random/uuid)
  CONNECTOR_ANALYSIS_ID=$(cat /proc/sys/kernel/random/uuid)
  SMTP_HOSTNAME=localhost
  EOF
  ) > .env
  ```
  `sudo sysctl -w vm.max_map_count=1048575`
  ### Chạy OpenCTI
  `sudo systemctl start docker.service`
  
  `docker-compose up -d`
  ### Sử dụng Docker swarm
  `docker swarm init`
  ### Đặt vào biến môi trường /etc/environment
  ```
  sudo cat .env >> /etc/environment
  sudo bash -c 'cat .env >> /etc/environment'
  sudo docker stack deploy --compose-file docker-compose.yml opencti
  ```
  >Lên Web: http://[ip]:8080

  Tài khoản mật khẩu trong file .env
  
  Tải khoản: OPENCTI_ADMIN_EMAIL
  
  Mật khẩu: OPENCTI_ADMIN_PASSWORD
  

  
