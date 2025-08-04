## extract_proxy

```cpp
// 初始化提取服务  
int VideoProcessor::Init(const std::string& fp_name, const std::string& server_addr) {  
  TRPC_LOG_INFO("Create service proxy. fp_name:" << fp_name << " server_addr:" << server_addr);  
  // sift提取老协议，其他提取重构后统一协议  
  if (std::string::npos != fp_name.find("sift_")) {  
    SiftExtractPtr sift_ptr = trpc_client_->GetProxy<  
                      sift_extract::AccextractServiceProxy>(server_addr);  
    sift_ptr_map_[fp_name] = sift_ptr;  
  } else {  
    CommExtractPtr comm_ptr = trpc_client_->GetProxy<  
                      fp_extract::ExtractServiceProxy>(server_addr);  
    commfp_ptr_map_[fp_name] = comm_ptr;  
    commfp_addr_map_[fp_name] = server_addr;  
  }  
  return 0;  
}
```
```cpp
CommExtractPtr comm_ptr = trpc_client_->GetProxy<fp_extract::ExtractServiceProxy>(server_addr); 
```
## TrpcClient

```cpp
class TrpcClient {  
 public:  
  /// @brief 根据名称获取一个服务代理对象,  
  ///        如果option不为空，优先使用option中设置的value(优先级>配置文件配置项)  
  ///        option由用户释放，使用&取变量地址符传入即可  
  ///        设计上serviceproxy只提供默认的只读的属性，在调用GetProxy获取时确定，需要动态改变的使用context设置  
  ///        设置overwrite=true时会重新创建并返回新的代理对象，原有对象的option不会被修改  
  template <typename T>  
  std::shared_ptr<T> GetProxy(const std::string& name, const ServiceProxyOption* option = nullptr,  
                              bool overwrite = false);  

 private:  
  ClientConfig client_conf_;  
  
  ServiceProxyManager service_proxy_manager_;  
};  
  
template <typename T>  
std::shared_ptr<T> TrpcClient::GetProxy(const std::string& name, const ServiceProxyOption* option, bool overwrite) {  
  return service_proxy_manager_.GetProxy<T>(name, client_conf_, option, overwrite);  
}  
  
/// @brief 用于获取全局的TrpcClient(调用前请先初始化框架配置和插件)  
/// @note 全局的TrpcClient的退出动作在UnregisterPlugins中调用，业务不需要主动调用  
std::shared_ptr<TrpcClient> GetTrpcClient();  
  
}  // namespace trpc
```

### GetTrpcClient
```cpp
std::shared_ptr<TrpcClient> GetTrpcClient() {  
  static std::shared_ptr<TrpcClient> client =  
      std::make_shared<TrpcClient>(TrpcConfig::GetInstance()->GetClientConfig());  
  return client;  
}
```
## ClientConfig
```cpp
struct ClientConfig {  
  // 请求超时时长，已失效  
  uint32_t timeout = kDefaultTimeout;  
  
  // 请求队列大小，已失效  
  uint32_t req_queue_size = kDefaultReqQueueSize;  
  
  // 客户端IO线程数，已失效  
  uint32_t io_thread_num = kDefaultIOThreadNum;  
  
  // 客户端处理线程数，已失效  
  uint32_t handle_thread_num = kDefaultHandleThreadNum;  
  
  // 连接空闲超时时间，已失效  
  uint32_t connect_idel_timeout = kDefaultIdleTime;  
  
  // ServiceProxy配置集合  
  std::vector<ServiceProxyConfig> service_proxy_config;  
  
  std::vector<std::string> filters;  
  
  struct ConnLimiterConfig conn_limiter;  
  
  void Display() const;  
};
```
## ServiceProxyConfig
```cpp
struct ServiceProxyConfig {  
  // 被调Service的名称  
  // 规范格式: trpc.应用名.服务名.pb的service名[.接口名]  
  std::string name;  
  
  std::string namespace_;  
  
  // 请求使用的协议  
  std::string protocol = kDefaultProtocol;  
  
  // 被请求Service的网络类型  
  // 目前包括: tcp/udp/...  
  std::string network = kDefaultNetwork;  
  
  // 连接类型  
  std::string conn_type = kDefaultConnType;  
  
  // 请求的超时时间，单位为毫秒  
  uint32_t timeout = kDefaultTimeout;  
  
  // Serviceproxy使用的selector名称  
  std::string selector_name;  
  
  // service 使用的future transport名称，已废弃  
  std::string future_transport_name = kDefaultFutureTransportName;  
  
  // service 使用的协程 transport名称，已废弃  
  std::string coroutine_transport_name = kDefaultCoroutineTransportName;  
  
  // 路由名称  
  std::string target;  
  
  // 被调service名称，用于监控上报及tracing上报  
  // 为空的话，调用TrpcClient GetProxy后会初始化为proxy的name  
  std::string callee_name;  
  
  // 被调的set全名(用于指定调用哪个set下的服务)  
  std::string callee_set_name;  
  
  // 使用的负载均衡插件名称, 默认为空(表示使用框架提供的默认负载均衡算法)  
  std::string load_balance_name;  
  
  // 负载均衡插件类型，用于一个负载均衡插件支持多种类型的情况  
  std::string load_balance_type;  
  
  // 是否禁用服务规则路由  
  bool disable_servicerouter = kDefaultDisableServiceRouter;  
  
  // 新架构新增字段  
  // 是否使用连接复用  
  bool is_conn_complex = kDefaultIsConnComplex;  
  
  // 发送的最大包长  
  uint32_t max_packet_size = kDefaultMaxPacketSize;  
  
  // 接收缓冲区长度  
  uint32_t recv_buffer_size = kDefaultRecvBufferSize;  
  
  // 发送队列容量大小  
  uint32_t send_queue_capacity = kDefaultSendQueueCapacity;  
  
  // 一个后端节点可建立的最大连接数  
  uint32_t max_conn_num = kDefaultMaxConnNum;  
  
  // 访问一个后端节点时，预热的连接数, 这个值小于等于max_conn_num  
  uint32_t pre_warm_conn_num = kDefaultPreWarmConnNum;  
  
  // 连接空闲超时时间  
  uint32_t idle_time = kDefaultIdleTime;  
  
  // 请求超时检测间隔，单位ms  
  uint32_t request_timeout_check_interval = kDefaultRequestTimeoutCheckInterval;  
  
  // 连接复用模式下，空闲连接超时后，断开是否需要重连接  
  bool is_reconnection = false;  
  
  // 是否支持pipeline  
  bool support_pipeline = kDefaultSupportPipeline;  
  
  // 连接超时时长，单位为ms，默认值为0，表示不检查连接超时  
  uint32_t connect_timeout = 0;  
  
  // 是否支持连接断开后重连(在请求发送时校验), 默认支持，用于固定连接场景  
  // 如果不支持重连的话，当固定连接断开后，在此连接上发包将会失败；反之则会重新连接后再发包  
  // 事务场景（或者其他不需要重连的场景）需要设置为false  
  bool allow_reconnect = true;  
  
  // 采用writev时一次写入的缓冲区长度  
  uint32_t merge_send_data_size = kDefaultMergeSendDataSize;  
  
  // 存储ip/port <--> Connector关系的hashmap的桶大小  
  uint32_t endpoint_hash_bucket_size = kEndpointHashBucketSize;  
  
  // 使用的transport(保留便于以后扩展)  
  std::string transport_plugin_name = kDefaultTransportPluginName;  
  
  // 线程模型类型名称，已废弃  
  std::string threadmodel_type = kDefaultThreadmodelType;  
  
  // 线程模型实例名称  
  std::string threadmodel_instance_name = "";  
  
  // service级别的filter集合  
  std::vector<std::string> service_filters;  
  
  // redis鉴权配置信息  
  RedisClientConf redis_conf;  
  
  // SSL/TLS config  
  ClientSslConfig ssl_config;  
  
  // Retry and hedging config  
  RetryHedgingConfig retry_hedging_config;  
  
  // service filter的配置, 其中key为filter插件名  
  std::map<std::string, std::any> service_filter_configs;  
  
  /// 流式流控窗口大小，单位：字节数，默认为65535，为0时代表关闭流控（目前对trpc流式生效，为本端接收窗口大小）  
  uint32_t stream_max_window_size = 65535;  
  
  /// 流空闲超时时间，单位：毫秒，默认为0，为0时代表不检测流是否空闲超时（目前对trpc流式生效）  
  uint32_t stream_idle_time = 0;  
  
  /// FiberPipeline下Connector的无锁队列大小，注意不建议调大，否则很容易占用较大内存  
  uint32_t fiber_pipeline_connector_queue_size = 8 * 1024;  
  
  /// Fiber链接池下空闲队列分片组个数；  
  /// 此值越大分配的链接会偏多，带来更好的并行度，会提升性能，但是会带来更多的链接；  
  /// 如果对创建连接数较为敏感可以考虑调小此值，如设置为1  
  uint32_t fiber_connpool_shards = 4;  
  
  void Display() const;  
};
```
## TrpcConfig
```cpp
area = "client";  
if (ConfigHelper::GetInstance()->IsNodeExist({area})) {  
  if (ConfigHelper::GetInstance()->GetConfig({area}, client_config_)) {  
    client_config_.Display();  
  } else {  
    TRPC_LOG_ERROR("Parse Area error:" << area);  
    return -1;  
  }  
}
```

### convert<trpc::ServiceProxyConfig>
```cpp
 static bool decode(const YAML::Node& node, trpc::ServiceProxyConfig& proxy_config) {  
    if (node["name"]) proxy_config.name = node["name"].as<std::string>();  
    if (node["target"]) proxy_config.target = node["target"].as<std::string>();  
    if (node["namespace"]) proxy_config.namespace_ = node["namespace"].as<std::string>();  
    if (node["protocol"]) proxy_config.protocol = node["protocol"].as<std::string>();  
    if (node["network"]) proxy_config.network = node["network"].as<std::string>();  
    if (node["conn_type"]) proxy_config.conn_type = node["conn_type"].as<std::string>();  
    if (node["timeout"]) proxy_config.timeout = node["timeout"].as<uint32_t>();  
    if (node["selector_name"]) proxy_config.selector_name = node["selector_name"].as<std::string>();  
    if (node["future_transport_name"])  
      proxy_config.future_transport_name = node["future_transport_name"].as<std::string>();  
    if (node["coroutine_transport_name"])  
      proxy_config.coroutine_transport_name = node["coroutine_transport_name"].as<std::string>();  
    if (node["callee_name"]) proxy_config.callee_name = node["callee_name"].as<std::string>();  
    if (node["callee_set_name"]) proxy_config.callee_set_name = node["callee_set_name"].as<std::string>();  
    if (node["load_balance_name"]) proxy_config.load_balance_name = node["load_balance_name"].as<std::string>();  
    if (node["load_balance_type"]) proxy_config.load_balance_type = node["load_balance_type"].as<std::string>();  
    if (node["disable_servicerouter"]) proxy_config.disable_servicerouter = node["disable_servicerouter"].as<bool>();  
  
    // 新架构新增字段  
    if (node["is_conn_complex"]) proxy_config.is_conn_complex = node["is_conn_complex"].as<bool>();  
    if (proxy_config.protocol == "http") proxy_config.max_packet_size = trpc::kDefaultHttpMaxPacketSize;  
    if (node["max_packet_size"]) proxy_config.max_packet_size = node["max_packet_size"].as<uint32_t>();  
    if (node["recv_buffer_size"]) proxy_config.recv_buffer_size = node["recv_buffer_size"].as<uint32_t>();  
    if (node["send_queue_capacity"]) proxy_config.send_queue_capacity = node["send_queue_capacity"].as<uint32_t>();  
    if (node["max_conn_num"]) proxy_config.max_conn_num = node["max_conn_num"].as<uint32_t>();  
    if (node["pre_warm_conn_num"]) proxy_config.pre_warm_conn_num = node["pre_warm_conn_num"].as<uint32_t>();  
    if (node["idle_time"]) proxy_config.idle_time = node["idle_time"].as<uint32_t>();  
    if (node["request_timeout_check_interval"]) {  
      auto interval = node["request_timeout_check_interval"].as<uint32_t>();  
      proxy_config.request_timeout_check_interval = interval > 0 ? interval : trpc::kDefaultRequestTimeoutCheckInterval;  
    }  
    if (node["is_reconnection"]) proxy_config.is_reconnection = node["is_reconnection"].as<bool>();  
    if (node["connect_timeout"]) proxy_config.connect_timeout = node["connect_timeout"].as<uint32_t>();  
    if (node["allow_reconnect"]) proxy_config.allow_reconnect = node["allow_reconnect"].as<bool>();  
    if (node["merge_send_data_size"]) proxy_config.merge_send_data_size = node["merge_send_data_size"].as<uint32_t>();  
    if (node["endpoint_hash_bucket_size"])  
      proxy_config.endpoint_hash_bucket_size = node["endpoint_hash_bucket_size"].as<uint32_t>();  
    if (node["transport_plugin_name"])  
      proxy_config.transport_plugin_name = node["transport_plugin_name"].as<std::string>();  
    if (node["threadmodel_type"]) proxy_config.threadmodel_type = node["threadmodel_type"].as<std::string>();  
    if (node["threadmodel_instance_name"])  
      proxy_config.threadmodel_instance_name = node["threadmodel_instance_name"].as<std::string>();  
  
    if (node["filter"]) proxy_config.service_filters = node["filter"].as<std::vector<std::string>>();  
  
    if (node["redis"]) {  
      proxy_config.redis_conf = node["redis"].as<trpc::RedisClientConf>();  
      proxy_config.redis_conf.enable = true;  
    }  
  
    if (node["retry_hedging"]) {  
      proxy_config.retry_hedging_config = node["retry_hedging"].as<trpc::RetryHedgingConfig>();  
    }  
    
    // SSL/TLS config  
    if (node["ssl"]) {  
      proxy_config.ssl_config = node["ssl"].as<trpc::ClientSslConfig>();  
    }  
  
    if (node["threadmodel_instance_name"]) {  
      proxy_config.threadmodel_instance_name = node["threadmodel_instance_name"].as<std::string>();  
    }  
  
    if (node["support_pipeline"]) {  
      proxy_config.support_pipeline = node["support_pipeline"].as<bool>();  
    }  
  
    if (node["filter_config"]) {  
      // 重试对冲限流策略  
      if (node["filter_config"][trpc::kRetryHedgingLimitFilter]) {  
        auto retry_hedging_config =  
            node["filter_config"][trpc::kRetryHedgingLimitFilter].as<trpc::RetryHedgingLimitConfig>();  
        proxy_config.service_filter_configs[trpc::kRetryHedgingLimitFilter] = retry_hedging_config;  
      }  
  
      if (node["filter_config"][trpc::kRateBasedRetryHedgingLimitFilter]) {  
        auto rate_based_retry_hedging_config =  
            node["filter_config"][trpc::kRateBasedRetryHedgingLimitFilter].as<trpc::RateBasedRetryHedgingLimitConfig>();  
        proxy_config.service_filter_configs[trpc::kRateBasedRetryHedgingLimitFilter] = rate_based_retry_hedging_config;  
      }  
    }  
  
    if (node["stream_max_window_size"]) {  
      proxy_config.stream_max_window_size = node["stream_max_window_size"].as<uint32_t>();  
    }  
  
    if (node["stream_idle_time"]) {  
      proxy_config.stream_idle_time = node["stream_idle_time"].as<uint32_t>();  
    }  
  
    if (node["fiber_pipeline_connector_queue_size"]) {  
      proxy_config.fiber_pipeline_connector_queue_size = node["fiber_pipeline_connector_queue_size"].as<uint32_t>();  
    }  
  
    if (node["fiber_connpool_shards"]) {  
      proxy_config.fiber_connpool_shards = node["fiber_connpool_shards"].as<uint32_t>();  
    }  
  
    return true;  
  }  
};
```

### convert<trpc::ClientConfig>
```cpp
static bool decode(const YAML::Node& node, trpc::ClientConfig& client_config) {  
  // if(node["caller"])  
  //   client_config.caller = node["caller"].as<std::string>();  if (node["req_queue_size"]) client_config.req_queue_size = node["req_queue_size"].as<uint32_t>();  
  if (node["timeout"]) client_config.timeout = node["timeout"].as<uint32_t>();  
  
  if (node["io_thread_num"]) client_config.io_thread_num = node["io_thread_num"].as<uint32_t>();  
  
  if (node["handle_thread_num"]) client_config.handle_thread_num = node["handle_thread_num"].as<uint32_t>();  
  
  if (node["service"]) {  
    for (size_t idx = 0; idx < node["service"].size(); ++idx) {  
      auto item = node["service"][idx].as<trpc::ServiceProxyConfig>();  
      client_config.service_proxy_config.push_back(item);  
    }  
  }  
  
  auto filter = node["filter"];  
  if (filter) {  
    for (auto&& idx : filter) {  
      client_config.filters.push_back(idx.as<std::string>());  
    }  
  }  
  
  if (node["conn_limiter"]) client_config.conn_limiter = node["conn_limiter"].as<trpc::ConnLimiterConfig>();  
  
  if (node["connect_idel_timeout"]) {  
    client_config.connect_idel_timeout = node["connect_idel_timeout"].as<uint32_t>();  
  }  
  
  return true;  
}
```
## ServiceProxyManager

```cpp
class ServiceProxyManager {  
 public:  
  /// @brief 线程安全的方式获取proxy  
  ///        支持增量设置proxy option, 优先级: 接口设置值 > 配置文件 > 框架内部默认值  
  template <typename T>  
  std::shared_ptr<T> GetProxy(const std::string& name, const ClientConfig& conf,  
                              const ServiceProxyOption* option_ptr = nullptr, bool overwrite = false);  
  
 private:  
  void InitProxy(const ServiceProxyConfig& proxy_conf, const std::shared_ptr<ServiceProxyOption>& option);  
  
 private:  
  concurrency::LightlyConcurrentHashMap<std::string, std::shared_ptr<ServiceProxy>> service_proxies_;  
  
};  
  
template <typename T>  
std::shared_ptr<T> ServiceProxyManager::GetProxy(const std::string& name, const ClientConfig& conf,  
                                                 const ServiceProxyOption* option_ptr, bool overwrite) {  
  if (TRPC_UNLIKELY(name.empty())) {  
    TRPC_FMT_CRITICAL("GetProxy failed, name is empty.");  
    return nullptr;  
  }  
  
  std::shared_ptr<ServiceProxy> proxy;  
  if (!overwrite && service_proxies_.Get(name, proxy)) {  
    return std::dynamic_pointer_cast<T>(proxy);  
  }  
  
  std::shared_ptr<T> new_proxy(new T());  
  
  auto option = std::make_shared<ServiceProxyOption>();  
  
  auto iter = std::find_if(conf.service_proxy_config.begin(), conf.service_proxy_config.end(),  
                           [&](const ServiceProxyConfig& proxy_conf) { return proxy_conf.name == name; });  
  
  if (option_ptr) {  
    // option优先级: 接口设置的值 > 配置文件 > 默认值  
    // 设置默认值  
    detail::SetDefaultOption(option);  
  
    // 设置配置文件中的值  
    if (iter != conf.service_proxy_config.end()) {  
      InitProxy(*iter, option);  
    }  
  
    // 设置option_ptr中的指定的非默认值  
    detail::SetSpecifiedOption(option_ptr, option);  
    if (option->threadmodel_type == ThreadModelType::FIBER) {  
      // 兼容旧的枚举逻辑  
      option->threadmodel_type_name = kFiberThreadmodelType;  
    }  
  } else {  
    if (iter != conf.service_proxy_config.end()) {  
      InitProxy(*iter, option);  
    } else {  
      // 使用默认配置初始化option  
      ServiceProxyConfig proxy_config;  
      InitProxy(proxy_config, option);  
    }  
  }  
  
  // option的name和GetProxy的name参数一致  
  option->name = name;  
  
  // 未设置name_space的话，命名空间和global保持一致  
  option->name_space = option->name_space.empty() ? trpc::TrpcConfig::GetInstance()->GetGlobalConfig().env_namespace  
                                                  : option->name_space;  
  
  // 未设置callee_name的情况下，callee_name设置成name  
  option->callee_name = option->callee_name.empty() ? name : option->callee_name;  
  
  // 如果被调路由名称没有设置的话, 则等于GetProxy传入的服务名  
  option->target = option->target.empty() ? name : option->target;  
  
  // 如果target是ip:port形式的话，则自动把selector_name设置为direct  
  if (util::IsValidIpPorts(option->target) && option->selector_name != "direct") {  
    option->selector_name = "direct";  
  }  
  
  // 缺省的caller_name设置为trpc.{app}.{server}  
  if (option->caller_name.empty()) {  
    option->caller_name = "trpc." + TrpcConfig::GetInstance()->GetServerConfig().app + "." +  
                          TrpcConfig::GetInstance()->GetServerConfig().server;  
  }  
  
  new_proxy->SetServiceProxyOptionInner(option);  
  
  // 依赖`new_proxy->SetServiceProxyOptionInner`先执行，并更新selector_name  
  if (option->selector_name == "direct" || option->selector_name == "domain") {  
    new_proxy->SetEndpointInfo(option->target);  
  }  
  
  TRPC_FMT_TRACE("ServiceProxy name:{}, target:{}, threadmodel_instance_name:{}", name, new_proxy->option_->target,  
                 new_proxy->option_->threadmodel_instance_name);  
  
  if (option->pre_warm_conn_num > 0) {  
    new_proxy->PreWarm();  
  }  
  
  if (overwrite) {  
    service_proxies_.InsertOrAssign(name, std::static_pointer_cast<ServiceProxy>(new_proxy));  
    return new_proxy;  
  }  
  
  if (!service_proxies_.GetOrInsert(name, std::static_pointer_cast<ServiceProxy>(new_proxy), proxy)) {  
    return new_proxy;  
  }  
  return std::dynamic_pointer_cast<T>(proxy);  
}  
  
}  // namespace trpc
```