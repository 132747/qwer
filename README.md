- name: Wait connection
  wait_for_connection:
    sleep: 1
    timeout: "30"
# 确保安装nginx
- name: Ensure nginx is at the latest version
  package:
    name: nginx
    state: latest
  register: ensure_nginx
  until: ensure_nginx is success
# 创建nginx80端口映射的工作目录
- name: create path
  file:
    path: /var/www/html
    state: directory
# 负责一个index.html文件
- name: Copy index.html
  template:
    src: ../templates/index.html.j2
    dest: /var/www/html/index.html
    mode: '0644'
# 复制一个php网页
- name: Copy index.php
  copy:
    src: ../templates/index.php
    dest: /var/www/html/index.php
    mode: '0644'
# 复制配置文件
- name: Copy nginx.conf
  template:
    src: ../templates/nginx.conf.j2
    dest: /etc/nginx/nginx.conf
    mode: '0644'
# 启动nginx
- name: Start nginx
  service:
    name: nginx
    state: started
