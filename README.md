
SiQA: A Large Multi-Modal Question Answering Model for Structured Images Based on RAG
====================

**SCQA** contains 1,000 organizational charts of listed Chinese companies and 5,112 questions that require understanding of the images to be answered.

We are currently waiting for the review results. We provide sample for your reference. **Thank you for your understanding.**


## Example

![alt text](https://github.com/a824705518/OCQA/raw/main/example/images/000436.jpg)

### Si2KG

We annotate the components in a structured image to generate a Knowledge Graph(KG) representing their meanings.

```bash
[{"image": "000436.jpg", "annotations": [
 {"label": "textbox", "coordinates": {"x": 754.0, "y": 144.0, "width": 118.0, "height": 32.0}},
 {"label": "textbox", "coordinates": {"x": 616.0, "y": 144.0, "width": 118.0, "height": 32.0}},
 {"label": "textbox", "coordinates": {"x": 889.5, "y": 144.0, "width": 119.0, "height": 32.0}},
 {"label": "textbox", "coordinates": {"x": 309.5, "y": 229.5, "width": 139.0, "height": 27.0}},
 {"label": "textbox", "coordinates": {"x": 79.5, "y": 144.0, "width": 129.0, "height": 32.0}},
 {"label": "arrow", "coordinates": {"x": 381.0, "y": 291.0, "width": 12.0, "height": 36.0}},
 {"label": "arrow", "coordinates": {"x": 75.5, "y": 207.5, "width": 13.0, "height": 27.0}},
 ...]}]
```
### QA data
We adopt `RoBERTa` as our encoder to develop our TagOp and use the following commands to prepare RoBERTa model 

```bash
[{
    "uid": "image_000436",
    "questions":[
{"order": 1,"question": "中钢天源的下属二级单位都有哪几个？","answer": ["磁材厂","钕铁硼厂"],"answer_from": "Directly","facts": ["磁材厂","钕铁硼厂"],"calculate":0},
{"order": 2,"question": "中钢天源拥有贵州金瑞多少的股份？","answer": ["26.5%"],"answer_from": "Directly","facts": ["26.5%"],"calculate":0},
{"order": 3,"question": "中钢天源的全资子公司都有哪几个？","answer": ["湖南特材","通力公司","南京研究院","中唯公司","国知新材料","南京新材料"],"answer_from": "Directly","facts": ["湖南特材","通力公司","南京研究院","中唯公司","国知新材料","南京新材料"],"calculate":0},
{"order": 4,"question": "中钢天源控制天源智能的多少股份？","answer": ["54.30%"],"answer_from": "Directly","facts": ["54.30%"],"calculate":0},
{"order": 5,"question": "中钢天源的控股子公司共有几个？","answer": ["3"],"answer_from": "Indirectly","facts": ["金宁三环","中钢制品院","天源智能"],"calculate":1},
{"order": 6,"question": "研发中心由哪个部门进行管理？","answer": ["中钢天源"],"answer_from": "Directly","facts": ["中钢天源"],"calculate":0}
]}]
```

            
### Any Questions?

If you have any questions, please email the author Jiawang Liu at [liujiawang@mails.qust.edu.cn], thank you.
