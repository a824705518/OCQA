
SiQA: A Large Multi-Modal Question Answering Model for Structured Images Based on RAG
====================

**SCQA** contains 1,000 organizational charts of listed Chinese companies and 5,112 questions that require understanding of the images to be answered.

We are currently waiting for the review results. We provide sample for your reference. **Thank you for your understanding.**


## Example

![alt text](https://github.com/a824705518/OCQA/raw/main/example/images/000436.jpg)

### Si2KG

We annotate the components in a structured image to generate a Knowledge Graph(KG).

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

### Training & Testing

#### Preprocessing dataset

We heuristicly generate the "facts" and "mapping" fields based on raw dataset, which are stored under the folder of `dataset_tagop`.


#### Prepare dataset

```bash
PYTHONPATH=$PYTHONPATH:$(pwd):$(pwd)/tag_op python tag_op/prepare_dataset.py --mode [train/dev/test]
```

Note: The result will be written into the folder `./tag_op/cache` default.

#### Train & Evaluation 
```bash
CUDA_VISIBLE_DEVICES=2 PYTHONPATH=$PYTHONPATH:$(pwd) python tag_op/trainer.py --data_dir tag_op/cache/ \
--save_dir ./checkpoint --batch_size 48 --eval_batch_size 8 --max_epoch 50 --warmup 0.06 --optimizer adam --learning_rate 5e-4 \
--weight_decay 5e-5 --seed 123 --gradient_accumulation_steps 4 --bert_learning_rate 1.5e-5 --bert_weight_decay 0.01 \
--log_per_updates 50 --eps 1e-6 --encoder roberta
```

#### Testing
```bash
CUDA_VISIBLE_DEVICES=2 PYTHONPATH=$PYTHONPATH:$(pwd) python tag_op/predictor.py --data_dir tag_op/cache/ --test_data_dir tag_op/cache/ \\
--save_dir tag_op/ --eval_batch_size 32 --model_path ./checkpoint --encoder roberta
```

Note: The training process may take around 2 days using a single 32GB v100.

#### Checkpoint
You may download this checkpoint of the trained TagOp model vai [TagOp Checkpoint](https://drive.google.com/file/d/1Ttyh1xyulsGcOt_JmFsAhPuxx7G3fyha/view?usp=share_link)


### Citation

__Please kindly cite our work if you use our dataset or codes, thank you.__
```bash
@inproceedings{zhu-etal-2021-tat,
    title = "{TAT}-{QA}: A Question Answering Benchmark on a Hybrid of Tabular and Textual Content in Finance",
    author = "Zhu, Fengbin  and
      Lei, Wenqiang  and
      Huang, Youcheng  and
      Wang, Chao  and
      Zhang, Shuo  and
      Lv, Jiancheng  and
      Feng, Fuli  and
      Chua, Tat-Seng",
    booktitle = "Proceedings of the 59th Annual Meeting of the Association for Computational Linguistics and the 11th International Joint Conference on Natural Language Processing (Volume 1: Long Papers)",
    month = aug,
    year = "2021",
    address = "Online",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2021.acl-long.254",
    doi = "10.18653/v1/2021.acl-long.254",
    pages = "3277--3287"
}
```
### License

The TAT-QA dataset is under the license of [Creative Commons (CC BY) Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/)
            
### Any Questions?

If you have any questions, please email the author Jiawang Liu at [liujiawang@mails.qust.edu.cn], thank you.
