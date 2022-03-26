# ARSeek: Identifying API Resource using Code and Discussion on Stack Overflow

It is not a trivial problem to collect API-relevant examples, usages, and mentions on venues such as Stack Overflow. It requires efforts to correctly recognize whether the discussion refers to the API method that developers/tools are searching for. The content of the Stack Overflow thread, which consists of both text paragraphs describing the involvement of the API method in the discussion and the code snippets containing the API invocation, may refer to the given API method. Leveraging this observation, we develop ARSeek, a context-specific algorithm to capture the semantic and syntactic information of the paragraphs and code snippets in a discussion. ARSeek combines a syntactic word-based score with a score from a predictive model fine-tuned from CodeBERT. ARSeek beats the state-of-the-art approach by 14% in terms of F1-score.

## 0. Download data
Create data/ folder [ARSeek_root]/. [ARSeek_root] is the root folder of this project.
```
$ mkdir [ARSeek_root]/data/
$ mkdir [ARSeek_root]/model/
```

Download data from [link](https://drive.google.com/file/d/16PPwQrpuDmEASTOSwtghNlyrL283SBtb/view?usp=sharing) and put into the data/ folder above.

Download `pytorch_model.bin` from [link](https://drive.google.com/file/d/1SrPYh3k3E9r-PKYY9rFm--QIkUgKiW_U/view?usp=sharing) and put the file into the model/ folder



## 1. Install Docker enviroment
```
$ docker compose up -d
$ docker exec -it ARSeek bash
```

## 2. Run DATYS
```
$ cd /app/ARSeek
$ python benchmark_datys.py
```

## 3. Run ARSeek
```
$ cd /app/ARSeek
$ python benchmark_ARSeek.py
```

## 3.1 Regenerate the API relevance embeddings by using trained model
```
pretrained_model=microsoft/codebert-base
test_model=model/pytorch_model.bin
output_dir=ARSeek/model/
source_length=512

python ARSeek/rel_cls/api_rel_cls.py --model_type roberta --model_name_or_path $pretrained_model \
                                    --load_model_path $test_model --output_dir $output_dir \
                                    --max_source_length $source_length
```