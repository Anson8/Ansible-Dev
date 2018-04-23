1. 创建两个docker镜像
    
      ip为：100.100.0.5，100.100.0.6

2. 将目标主机(非ansible机器)的公钥添加到（ansible主机）known_hosts
  
      ssh-keyscan 100.100.0.5 100.100.0.6 >> /root/.ssh/known_hosts
  
3. 生成管理主机得私钥和公钥
    
      ssh-keygen -t rsa -b 2048 -P '' -f /root/.ssh/id_rsa

4. 添加目标主机到主机清单中
  
    vi /etc/ansible/hosts

5. 配置palybook

    ``` ssh-addkey.yml 
    ---
    - hosts: all
      gather_facts: no
      tasks:
      - name: install ssh key
        authorized_key: user=root 
                        key="{{ lookup('file', '/root/.ssh/id_rsa.pub') }}" 
                        state=present
    ```

6. 运行playbook

    ansible-playbook ssh-addkey.yml --extra-vars hosts -k
  
    —extra-vars：通过命令传递参数
  
    -k：弹出输入登录密码

7. 验证
