# HyTE
## HyTE: Hyperplane-based Temporally aware Knowledge Graph Embedding

Source code and dataset for [EMNLP 2018](http://emnlp2018.org) paper: [HyTE: Hyperplane-based Temporally aware Knowledge Graph Embedding](http://talukdar.net/papers/emnlp2018_HyTE.pdf).

![](https://github.com/malllabiisc/HyTE/blob/master/time_proj.png)
*Overview of HyTE (proposed method). a temporally aware
KG embedding method which explicitly incorporates time in the entity-relation space by
stitching each timestamp with a corresponding hyperplane. HyTE not only performs KG
inference using temporal guidance, but also predicts temporal scopes for relational facts with missing time annotations. Please refer paper for more details.*
### Dependencies

* Compatible with TensorFlow 1.x and Python 3.x.
* Dependencies can be installed using `requirements.txt`.


### Dataset:

* Download the processed version of [WikiData and YAGO](https://drive.google.com/open?id=1S0dcMDXVZp8CFSCMojkBQI1gCva8Dm-0) datasets.
* Unzip the `.zip` file in `data` directory.
* Documents are originally taken from [YAGO](https://www.mpi-inf.mpg.de/departments/databases-and-information-systems/research/yago-naga/yago/) and [Wikidata](https://www.wikidata.org/wiki/Wikidata:Main_Page).


### Usage:

* After installing python dependencies from `requirements.txt`.
* `time_proj.py` contains TensorFlow (1.x) based implementation of HyTE (proposed method). 
* To start training:
  ```shell
  python time_proj.py -name yago_data_neg_sample_5_mar_10_l2_0.00 -margin 10 -l2 0.00 -neg_sample 5 -gpu 5 -epoch 2000 -data_type yago -version large -test_freq 5
  ```
*  Some of the important Available options include:
  ```shell
  '-data_type' default ='yago', choices = ['yago','wiki_data'], help ='dataset to choose'
	'-version',  default = 'large', choices = ['large','small','time','financial'], help = 'data version to choose'
	'-test_freq', 	 default = 25,   	type=int, 	help='testing frequency'
	'-neg_sample', 	 default = 5,   	type=int, 	help='negative samples for training'
	'-gpu', 	 dest="gpu", 		default='1',			help='GPU to use'
	'-name', 	 dest="name", 		help='Name of the run'
	'-lr',	 dest="lr", 		default=0.001,  type=float,	help='Learning rate'
	'-margin', 	 dest="margin", 	default=1,   	type=float, 	help='margin'
	'-batch', 	 dest="batch_size", 	default= 50000,   	type=int, 	help='Batch size'
	'-epoch', 	 dest="max_epochs", 	default= 5000,   	type=int, 	help='Max epochs'
	'-l2', 	 dest="l2", 		default=0.0, 	type=float, 	help='L2 regularization'
	'-seed', 	 dest="seed", 		default=1234, 	type=int, 	help='Seed for randomization'
	'-inp_dim',  dest="inp_dim", 	default = 128,   	type=int, 	help='')
	'-L1_flag',  dest="L1_flag", 	action='store_false',   	 	help='Hidden state dimension of FC layer'
   ```

### Evaluation: 
* Validate after Training. 
* Use the same model name and test frequency used at training as arguments for the following evalutation--
* For getting best validation MR and hit@10 for head and tail prediction:
 ```shell
    python result_eval.py -eval_mode valid -model yago_data_neg_sample_5_mar_10_l2_0.00 -test_freq 5
 ```
* For getting best validation MR and hit@10 for relation prediction:
```shell
   python result_eval_relation.py -eval_mode valid -model yago_data_neg_sample_5_mar_10_l2_0.00  -test_freq 5
```
The Evaluation run will output the **`Best Validation Rank`** and the corresponding **`Best Validation Epoch`** when it was achieved. Note them down for obtaining results on test set. 

### Testing:
* Test after validation using the best validation weights.
* First run the `time_proj.py` script once to restore parameters and then dump the predictions corresponding the the test set.
```shell
 python time_proj.py -res_epoch `Best Validation Epoch` -onlyTest -restore -name yago_data_neg_sample_5_mar_10_l2_0.00 -margin 10 -l2 0.00 -neg_sample 5 -gpu 0 -data_type yago -version large
```
* Now evaluate the test predictions to obtain MR and hits@10 using
```shell
python result_eval.py -eval_mode test -test_freq `Best Validation Epoch` -model yago_data_neg_sample_5_mar_10_l2_0.00
```


### Citing:

```tex
InProceedings{D18-1225,
  author = 	"Dasgupta, Shib Sankar
		and Ray, Swayambhu Nath
		and Talukdar, Partha",
  title = 	"HyTE: Hyperplane-based Temporally aware Knowledge Graph Embedding",
  booktitle = 	"Proceedings of the 2018 Conference on Empirical Methods in Natural Language Processing",
  year = 	"2018",
  publisher = 	"Association for Computational Linguistics",
  pages = 	"2001--2011",
  location = 	"Brussels, Belgium",
  url = 	"http://aclweb.org/anthology/D18-1225"
}
```
