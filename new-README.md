# OLAF

- An Open Source Federated Learning Simulation Platform
- IMC2020: [IMC2020](https://conferences.sigcomm.org/imc/2020/)
- paper: [TITLE]()



## What can OLAF simulate?

Briefly speaking, we develop OLAF to simulate federated learning process more in line with the fact. 



### Deadline

We add deadline setting for simulating failed downloading/uploading and time out training. Now deadline follows a normal distribution in each round, and each client has the same deadline in one round. You can set the deadline's normal distribution parameters in the config file.



### Heterogeneity

#### Hardware Heterogeneity

Each client is bundled with a device type. Each device type has different training speeds and network speeds. We also support self-defined device type(-1) whose parameter you can set in the code manually for more complexed simulation. Note that if a client's device is not specified i.e. None, the program will use real training time instead of the simulation time, which is not recommended.

The source code for measure the on-device training time is available in the [android](android/) directory



#### Behavior Heterogeneity

Each client is bundled with a timer, which is bundled with one trace. Timer gets the available time according to [google definition](https://arxiv.org/pdf/1902.01046.pdf). OLAF will run in ideal mode if trace file is not found or `behav_hete` is set to `False`



#### Statistic Heterogeneity

- data in each client is non-i.i.d
- you can set `max_sample` to control the max sample number in each client



### Round Failure

In federated settings, if there are not enough devices to upload the results in a round, then this round will be regarded as a failed round and the global model will not be updated. To simulate it, we add a update_frac parameter. If the uploaded fraction is smaller than update_frac, then this round will fail. You can also set it in the config file.



### Config

To simplify the command line arguments, we move most of the parameters to a config file. Also, we add some other parameters as put above for better simulation. Here are some details.

```bash
# line started with # (commented) will be ignored
behav_hete True 
# bool, whether to simulate behavior heterogeneity
hard_hete True
# bool, whether to simulate hardware heterogeneity, which contains differential on-device training time and network speed
no_training False 
# bool, whether to run in no_training mode, skip training process if True
real_world False
# bool, whether to run read-world DL dataset
dataset femnist 
# dataset to use
model cnn 
# file that defines the DNN model
num_rounds 500 
# number of FL rounds to run
learning_rate 0.01 
# learning-rate of DNN
eval_every 5
# evaluate every # rounds, -1 for not evaluate
clients_per_round 100 
# expected clients in each round
min_selected 60 
# min selected clients number in each round, fail if not satisfied
max_sample 340 
# number of max sampleto use in each selected client
batch_size 10
# batch-size for training 
num_epochs 5 
# number epochs imn each client in each round
seed 0 
# basic random seed
round_ddl 270 0 
# μ and σ for deadline, which follows a normal distribution
update_frac 0.8  
# min update fraction in each round, round succeeds only when fraction of succeeded client not less than #
max_client_num -1
# 

# NOTE! [aggregate_algorithm, fedprox*, structure_k, qffl*] is mutually-exclusive
aggregate_algorithm SucFedAvg 
## choose in [SucFedAvg, FedAvg], please refer to models/server.py for more details
# compress_algo grad_drop
## gradiant compress algorithm, choose in [grad_drop, sign_sgd], not use if commented
fedprox True
fedprox_mu 0.5
fedprox_active_frac 0.8
## whether to apply fedprox and params needed, please refer to the sysml'20 for more details
# structure_k 100
## the k for structured update, not use if commented, please refer to the arxiv for more 
# qffl True
# qffl_q 5
## whether to apply qffl and params needed, please refer to the ICLR'20 for more
```



## How to run it

### example

```bash
git clone https://github.com/imc20submission/olaf.git
pip3 install -r requirements.txt
# download data, modify code if needed, refer to Chapter.Dataset for more details
cd models/
python3 main.py [--config yourconfig.cfg]
# use --config option to specify the config-file, default.cfg will be used if not specified
# the output log is CONFIG_FILENAME.log
```



### results in paper

here is some examples for experiment result we reported in our paper

You can just modify the `models/default.cfg` and then run `python main.py`, here are some pieces of config details we used

- exp1
- exp2
- exp3



## Datasets

### DL Datasets

#### FEMNIST

- **Overview:** Image Dataset
- **Details:** 62 different classes (10 digits, 26 lowercase, 26 uppercase), images are 28 by 28 pixels (with option to make them all 128 by 128 pixels), 3500 users
- **Task:** Image Classification



#### Sentiment140

- **Overview:** Text Dataset of Tweets
- **Details**: 660120 users
- **Task:** Sentiment Analysis



#### Celeba

- **Overview:** Image Dataset based on the [Large-scale CelebFaces Attributes Dataset](http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)
- **Details:** 9343 users (we exclude celebrities with less than 5 images)
- **Task:** Image Classification (Smiling vs. Not smiling)



#### Reddit

- **Overview:** We preprocess the Reddit data released by [pushshift.io](https://files.pushshift.io/reddit/) corresponding to December 2017.
- **Details:** 1,660,820 users with a total of 56,587,343 comments. 
- **Task:** Next-word Prediction.



#### Read-World Dataset

- we are so sad that we cannot open the real-world dataset due to privacy enforcement



### User Behavior Trace

- you can download the user behavior trace data [here](https://drive.google.com/file/d/1oj-9byiw5r2iEIn9GburekJRKxtdFU7i/view?usp=sharing).

- modify the file path in [models/client.py](models/client.py), 

  i.e.  `with open('/path/to/user_behavior_trace.json', 'r', encoding='utf-8') as f: ` 



## Notes

- Install the libraries listed in ```requirements.txt```

  - i.e. with pip: run ```pip3 install -r requirements.txt```

- Go to directory of respective dataset `data/$DATASET` for instructions on generating data

- please consider to cite our paper if you use the code or data in your research project

  ```
  @inproceeding{
  }
  ```

  