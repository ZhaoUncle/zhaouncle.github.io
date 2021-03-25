# 【ELK】重置 elasticsearch 的超管 elastic 账号密码


<!--more-->



按照下述步骤创建本地超级账户，然后使用api接口本地超级账户重置elastic账户的密码

# 1. 没有忘记 elastic 的超管密码

直接通过 [api](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/security-api-change-password.html) 重置 elastic 超级管理员的密码

```rust
curl -H "Content-Type:application/json" -XPOST -u elastic 'http://127.0.0.1:9200/_xpack/security/user/elastic/_password' -d '{ "password" : "newpassword" }'
```

# 2. 忘记了 elastic 的超管密码

1. 确保你的配置文件中支持本地账户认证支持，如果你使用的是xpack的默认配置则无需做特殊修改；如果你配置了其他认证方式则需要确保[配置本地认证方式](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/configuring-security.html)在ES_HOME/config/elasticsearch.yml中；

2. 使用命令 [ES_HOME/bin/elasticsearch-users](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/users-command.html) 创建一个基于本地问价认证的超级管理员

```undefined
bin/elasticsearch-users useradd temp_admin -p temp_admin_passwd -r superuser
```

3.  通过 api 重置 elastic 超级管理员的密码

```rust
curl -u temp_admin -p temp_admin_passwd -XPUT 'http://127.0.0.1:9200/_xpack/security/user/elastic/_password' -H 'Content-Type: application/json' -d'
{
  "password" : "new_password"
}
```

4. 校验下密码是否重置成功

```rust
curl -u elastic 'http://localhost:9200/_xpack/security/_authenticate?pretty'
```

5. 如果你确定后续不再使用本地认证则可将elasticsearch.yml文件中的本地文件认证方式删除掉；


