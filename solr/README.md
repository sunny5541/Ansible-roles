# Ansible Role: solr

安装 solr

角色仿照：https://github.com/kuailemy123/Ansible-roles/tree/master/zookeeper  编写

## 介绍

Solr是一个流行的，快速的开源企业搜索平台，它来自Apache Lucene项目。 其主要功能包括强大的全文搜索，命中突出显示，分面搜索，动态聚类，数据库集成，丰富的文档（如Word，PDF）处理和地理空间搜索。 Solr具有高度的可扩展性，提供分布式搜索和索引复制功能，它为世界上许多最大的互联网站点提供搜索和导航功能。

官方地址：https://lucene.apache.org/solr/

## 要求

此角色仅在RHEL及其衍生产品上运行。

## 测试环境

- ansible 2.4.1.0
- os Centos 6.5 X64
- zookeeper 3.4.10
- solr 6.6.2

## 角色变量

    software_files_path: "/opt/software"
    software_install_path: "/usr/local"
    
    solr_version: "6.6.2"
    
    solr_file: "solr-{{ solr_version }}.tgz"
    solr_file_path: "{{ software_files_path }}/{{ solr_file }}"
    solr_file_url: "http://mirrors.shuosc.org/apache/lucene/solr/{{ solr_version }}/solr-{{ solr_version }}.tgz"
    
    zookeeper_hosts: ""
    
    solr_user: "solr"
    solr_port: 8983
    
    solr_home: "/var/solr"
    solr_dir: "{{ solr_home if zookeeper_hosts == '' else solr_home~'/'~solr_port}}"
    
    solr_env_file: "{{ solr_dir }}/solr.in.sh"
    solr_name: "solr{{ solr_port if zookeeper_hosts != '' else '' }}"
    solr_datadir: "{{ solr_dir }}/data"
    solr_datalogdir: "{{ solr_dir }}/logs"

## 依赖

java >= 1.8

## Example Playbook

安装solr单节点实例,默认端口 8983:

    ---
    
    - hosts: 192.168.112.171
    
      roles:
        - { role: solr }

单机伪分布式集群安装：

    ---
    
    - hosts: 192.168.112.171
    
      vars:
        zookeeper_hosts: "192.168.112.171:2181,192.168.112.171:2182,192.168.112.171:2183"
        
    
      roles:
        - { role: solr, solr_port: 8983 }
        - { role: solr, solr_port: 8984 }
        - { role: solr, solr_port: 8985 }
 
分布式安装：

    ---
    
    - hosts: 192.168.112.171, 192.168.112.174, 192.168.112.175
    
      vars:
        zookeeper_hosts: "192.168.112.171:2181,192.168.112.174:2181,192.168.112.175:2181"
        
    
      roles:
        - { role: solr }   

## 使用
    /etc/init.d/solr
    Usage: /etc/init.d/solr {start|stop|restart|status}


    启动命令：/etc/init.d/solr start 
    关闭命令：/etc/init.d/solr stop 
    查看状态命令：/etc/init.d/solr status 
