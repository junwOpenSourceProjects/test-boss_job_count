# boss_job_count
boss网站爬虫，查看java岗位数量

## 项目架构

```mermaid
graph TB
    subgraph 数据采集层
        A[Boss直聘网站] -->|爬取职位数据| B[NestJS爬虫服务]
        B -->|解析HTML| C[数据解析模块]
        C -->|结构化数据| D[职位实体Entity]
    end

    subgraph 数据存储层
        D -->|持久化存储| E[(MySQL数据库)]
        E -->|Docker容器| F[数据表job]
    end

    subgraph 技术栈
        G[TypeScript]
        H[NestJS框架]
        I[TypeORM]
        J[Docker]
    end

    style A fill:#e1f5fe
    style B fill:#4fc3f7
    style E fill:#81c784
    style F fill:#81c784
    style G fill:#fff9c4
    style H fill:#fff9c4
    style I fill:#fff9c4
    style J fill:#fff9c4
```

## 测试流程

```mermaid
sequenceDiagram
    participant U as 用户
    participant N as NestJS服务
    participant S as Boss网站
    participant D as MySQL数据库

    U->>N: npm run start:dev
    N->>S: 发送HTTP请求
    S-->>N: 返回职位HTML
    N->>N: 解析职位信息
    N->>D: 存储职位数据
    D-->>N: 确认存储成功
    N-->>U: 输出统计结果
```

## 快速开始

npm install

npm run start:dev

数据库使用的docker容器，需要先配置连接

sql文件如下：

-- auto-generated definition
create table job
(
    id      int auto_increment
        primary key,
    name    varchar(30)  not null comment '职位名称',
    area    varchar(20)  not null comment '区域',
    salary  varchar(10)  not null comment '薪资范围',
    link    varchar(600) not null comment '详情页链接',
    company varchar(30)  not null comment '公司名',
    `desc`  text         not null comment '职位描述'
);

