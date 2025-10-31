## Dig

> **Dig（Domain Information Groper）由 ISC（Internet Systems Consortium）的 BIND 项目团队于 1990 年代初期开发**，目的是为系统管理员提供一个比传统 nslookup 更强大、输出更详细的 DNS 查询和诊断工具。**它随 BIND 9 一起发布并逐渐成为 Unix/Linux 系统的标准 DNS 诊断工具**，因其灵活的查询选项和清晰的输出格式而被广泛采用。



### 概述

**Dig**（Domain Information Groper）是一个功能强大的**命令行 DNS 查询工具**，它的核心功能是通过域名（如`google.com`）查询对应的 IP 地址。它是 DNS 系统的 “命令行接口”，让你能直接在终端里查看域名解析的详细过程和结果。



### 示例

```shell
$ dig github.com

; <<>> DiG 9.18.1 <<>> github.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12345
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; QUESTION SECTION:
;github.com.                    IN      A

;; ANSWER SECTION:
github.com.             60      IN      A       20.205.243.166

;; Query time: 23 msec
;; SERVER: 192.168.1.1#53(192.168.1.1)
;; WHEN: Fri Oct 31 08:06:08 UTC 2025
;; MSG SIZE  rcvd: 55
```

**输出说明**：

- `status: NOERROR`：表示 DNS 查询**成功**，无错误。
- `flags: qr rd ra`：
  - `qr`：表示这是**响应包**（而非请求包）；
  - `rd`：表示发起了**递归查询**（请求 DNS 服务器帮我们完成全流程解析）；
  - `ra`：表示 DNS 服务器**支持递归**（能完成我们的递归请求）。
- `QUERY: 1, ANSWER: 1`：发起了 1 次查询，返回 1 条结果。
- `AUTHORITY: 0` 说明没有涉及权威服务器，`ADDITIONAL: 1` 说明有一条额外的辅助记录（常为 EDNS 配置）
- `QUESTION SECTION`：查询的问题（查询 github.com 的 A 记录，即 “将域名解析为 IPv4 地址” 的记录类型）

- `ANSWER SECTION`：查询结果（IP 地址 20.205.243.166，该解析结果在本地缓存中保留 **60 秒**，过期后需重新查询）
- `Query time`：查询耗时 23 毫秒，速度很快
- `SERVER`：表示查询发送到192.168.1.1这个 DNS 服务器，端口是 DNS 默认的 `53`，使用**UDP 协议**通信

### 主要功能

它能查询域名对应的 **IP 地址、邮件交换记录（MX）、名称服务器（NS）、权威服务器** 等各类 DNS 记录，帮助用户：

- **DNS 查询**：查询域名对应的 IP 地址
- **反向查询**：通过 IP 地址查询域名
- **记录类型查询**：查询 A、AAAA、MX、NS、TXT、CNAME 等各种 DNS 记录
- **DNS 服务器测试**：测试特定 DNS 服务器的响应
- **故障诊断**：排查 DNS 解析问题

简言之，`dig` 是深入理解和诊断 DNS 系统的 “专业探针”，通过它可以清晰掌握域名到 IP 的解析逻辑，高效解决各类 DNS 相关问题。