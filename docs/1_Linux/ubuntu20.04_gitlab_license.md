---
title: ubuntu20.04-架设Gitlab服务器-Licese
categories:
- [Linux,Gitlab]
tags:
- [ubuntu]
- [Gitlab]
date: 2023-11-15 18:00:00
updated: 2023-11-15 18:00:00
---

# Ubuntu20.04 架设Gitlab服务器-Licese



- [ubuntu20.04-架设Gitlab服务器篇为基础](https://szpzhy.com/2023/11/14/ubuntu20.04_gitlab/)



## 安装关联软件

```bash
sudo apt install ruby
```



## 下载安装Gitlab-license

```bash
sudo gem install gitlab-license
```



## 创建license生成文件

```bash
cd ~
vim license.rb
---

require "openssl"
require "gitlab/license"

key_pair = OpenSSL::PKey::RSA.generate(2048)
File.open("license_key", "w") { |f| f.write(key_pair.to_pem) }

public_key = key_pair.public_key
File.open("license_key.pub", "w") { |f| f.write(public_key.to_pem) }

private_key = OpenSSL::PKey::RSA.new File.read("license_key")
Gitlab::License.encryption_key = private_key

license = Gitlab::License.new
license.licensee = {
  "Name"    => "Users",
  "Company" => "GitLab",
  "Email"   => "users@gitlab.com"
}

license.starts_at         = Date.new(2024, 10, 04)
license.expires_at        = Date.new(2074, 10, 03)
license.notify_admins_at  = Date.new(2074, 10, 01)
license.notify_users_at   = Date.new(2074, 10, 01)
license.block_changes_at  = Date.new(2074, 11, 04)
license.restrictions  = {
  active_user_count: 10000
}

puts "License:"
puts license

data = license.export

puts "Exported license:"
puts data

File.open("GitLabBV.gitlab-license", "w") { |f| f.write(data) }

public_key = OpenSSL::PKey::RSA.new File.read("license_key.pub")
Gitlab::License.encryption_key = public_key

data = File.read("GitLabBV.gitlab-license")
$license = Gitlab::License.import(data)

puts "Imported license:"
puts $license

unless $license
  raise "The license is invalid."
end

if $license.restricted?(:active_user_count)
  active_user_count = 10000
  if active_user_count > $license.restrictions[:active_user_count]
    raise "The active user count exceeds the allowed amount!"
  end
end

if $license.notify_admins?
  puts "The license is due to expire on #{$license.expires_at}."
end

if $license.notify_users?
  puts "The license is due to expire on #{$license.expires_at}."
end

module Gitlab
  class GitAccess
    def check(cmd, changes = nil)
      if $license.block_changes?
        return build_status_object(false, "License expired")
      end
    end
  end
end

puts "This instance of GitLab Enterprise Edition is licensed to:"
$license.licensee.each do |key, value|
  puts "#{key}: #{value}"
end

if $license.expired?
  puts "The license expired on #{$license.expires_at}"
elsif $license.will_expire?
  puts "The license will expire on #{$license.expires_at}"
else
  puts "The license will never expire."
end

```



## 运行license.rb

```bash
ruby license.rb
生成
GitLabBV.gitlab-license
license_key
license_key.pub
三个文件
```



## 配置license

###  1. 替换license公钥

```bash
sudo rm -rf /opt/gitlab/embedded/service/gitlab-rails/.license_encryption_key.pub
sudo cp license_key.pub /opt/gitlab/embedded/service/gitlab-rails/.license_encryption_key.pub
```



### 2. 修改授权级别

```bash
sudo vim /opt/gitlab/embedded/service/gitlab-rails/ee/app/models/license.rb
  def plan
-    restricted_attr(:plan).presence || STARTER_PLAN
+    restricted_attr(:plan).presence || ULTIMATE_PLAN
  end
 
  def edition
```



### 3. 重新配置并重启服务

```bash
sudo gitlab-ctl reconfigure

sudo gitlab-ctl stop
sudo gitlab-ctl restart
```



### 4. 登陆Gitlab并设置

- 查看生成的许可证密钥并复制

```bash
cat GitLabBV.gitlab-license
```

- 登陆Gitlab进入管理界面(网址后面加/admin)
  - 菜单->管理员->设置->通用->添加许可证处点击展开->粘贴刚复制的许可证密钥
  - 菜单->管理员->订阅 (查看激活状态)