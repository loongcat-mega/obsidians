
```python
import json
import jsonlines
def convert_json_to_jsonl(json_file, jsonl_file):
    with open(json_file, 'r') as file:
        data = json.load(file)

    with jsonlines.open(jsonl_file, 'w') as writer:
        writer.write(data)
def convert_jsonl_to_json(jsonl_file, json_file):
    with jsonlines.open(jsonl_file, 'r') as reader:
        data = [obj for obj in reader]

    with open(json_file, 'w') as file:
        json.dump(data, file)
        
convert_json_to_jsonl('alpaca_gpt4_data_zh.json', 'alpaca_gpt4_data_zh.jsonl')
   

```