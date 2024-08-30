```shell
├── deploy.sh
├── internal
│   ├── biz  业务逻辑层，处理业务逻辑，字段 数据处理转换
│   ├── config 配置文件层
│   ├── constants 常量层
│   ├── dao 数据库操作层
│   ├── entity 实体层
│   ├── filter 过滤层
│   ├── po 数据持久化层
│   └── service 服务层
├── main.go
└── news_super_deletion
```



```shell

├── deploy.sh
├── internal
│   ├── biz
│   │   ├── article_operation_executor.go
│   │   ├── audit_flow_manager.go
│   │   ├── audit_flow_manager_test.go
│   │   ├── mock
│   │   │   ├── article_operation_executor_mock.go
│   │   │   ├── audit_flow_manager_mock.go
│   │   │   └── super_deleteion_manager_mock.go
│   │   ├── operation_info_construct.go
│   │   ├── super_deleteion_manager.go
│   │   └── super_deleteion_manager_test.go
│   ├── config
│   │   ├── app_info.go
│   │   ├── article_fields.go
│   │   ├── audit.go
│   │   ├── config.go
│   │   ├── config_test.go
│   │   ├── cos.go
│   │   ├── helper.go
│   │   └── third_party.go
│   ├── constants
│   │   └── constant.go
│   ├── dao
│   │   ├── databus.go
│   │   ├── databus_test.go
│   │   ├── db.go
│   │   ├── mock
│   │   │   ├── databusdao_mock.go
│   │   │   └── super_deletion_dao_mock.go
│   │   ├── super_deletion_dao.go
│   │   └── super_deletion_dao_test.go
│   ├── entity
│   │   ├── audit.go
│   │   ├── entity_test.go
│   │   └── operation_value.go
│   ├── filter
│   │   ├── auth.go
│   │   ├── log.go
│   │   └── rate_limit.go
│   ├── po
│   │   ├── audit_info.go
│   │   ├── audit_info_test.go
│   │   └── super_deletion_record.go
│   └── service
│       ├── super_deletion.go
│       └── super_deletion_test.go
├── main.go
└── news_super_deletion
```