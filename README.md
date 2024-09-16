
SiQA: A Large Multi-Modal Question Answering Model for Structured Images Based on RAG
====================

**SCQA** contains 1,000 organizational charts of listed Chinese companies and 5,112 questions that require understanding of the images to be answered.

We are currently waiting for the review results. We provide sample for your reference.


## Example

![alt text](https://github.com/a824705518/OCQA/raw/main/example/images/000436.jpg)

### Requirements

To create an environment with [MiniConda](https://docs.conda.io/en/latest/miniconda.html) and activate it.

```bash
conda create -n tat-qa python==3.7
conda activate tat-qa
pip install -r requirement.txt
pip install torch-scatter -f https://pytorch-geometric.com/whl/torch-1.7.0+${CUDA}.html
```

We adopt `RoBERTa` as our encoder to develop our TagOp and use the following commands to prepare RoBERTa model 

```bash
cd dataset_tagop
mkdir roberta.large && cd roberta.large
wget -O pytorch_model.bin https://s3.amazonaws.com/models.huggingface.co/bert/roberta-large-pytorch_model.bin
wget -O config.json https://s3.amazonaws.com/models.huggingface.co/bert/roberta-large-config.json
wget -O vocab.json https://s3.amazonaws.com/models.huggingface.co/bert/roberta-large-vocab.json
wget -O merges.txt https://s3.amazonaws.com/models.huggingface.co/bert/roberta-large-merges.txt
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

For any issues please create an issue [here](https://github.com/nextplusplus/tat-qa/issues) or kindly drop an email to the author: Fengbin Zhu [zhfengbin@gmail.com](mailto:zhfengbin@gmail.com), thank you.
