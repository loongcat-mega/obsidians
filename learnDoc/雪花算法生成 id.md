```cpp
// 结构为：  
// 0---0000000000 0000000000 0000000000 0000000000 0 --- 00000 ---00000 ---0000000000 00  
// 符号位：1bit 最高位是符号位，固定为 0。0 表示正，1 表示负。  
// 时间戳：41bit 毫秒级时间戳（41 位的长度可以使用 69 年）。  
// 标识位：5bit DataCenterId; 5bit WorkId. 两个标识位组合最多可支持部署 1024 个节点  
// 序列号：12bit 递增序列号，毫秒内生成的 ID 通过序列号表示唯一，12bit 每毫秒可产生 4096 个 ID
```
```cpp
uint64_t IdWorker::GetNextID() {  
  std::lock_guard<std::mutex> lock(mutex_);  
  
  uint64_t timestamp = NowInMSec();  
  if (last_timestamp_ == timestamp) { 
  	// 保证不会溢出 
    sequence_ = (sequence_ + 1) & sequence_mask_;  
  	// seq==0 意味着这一毫秒内的序列号已经用完（1 毫秒内最多 4096 个 ID），此时必须等到下一毫秒才能继续生成 ID
    if (sequence_ == 0) {  
      timestamp = WaitUntilNextMillis(last_timestamp_);  
    }  
  } else if (last_timestamp_ > timestamp) {  
    // 发生时间回退， 导致上次时间比现在时间大, 等待最新时间直到比上次的时间大，即等待时间回复正常
    timestamp = WaitUntilNextMillis(last_timestamp_);  
    sequence_ = 0;  
  } else {  
    sequence_ = 0;  
  }  
  
  last_timestamp_ = timestamp;  
  return  ((timestamp - twepoch_) << timestamp_left_shift_ |  
                        data_center_id_ << center_id_shift_ |  
                        worker_id_  << worker_id_shift_ |  
                        sequence_);  
}

WaitUntilNextMillis关键逻辑：
while (timestamp <= last_timestamp) {  
  timestamp = NowInMSec();  
}
```
