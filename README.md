

# Requirements
```
python >= 3.5.2
pytorch >= 1.0
sklearn >= 0.19.0
```

# Training commands-

## TimePlex (base):

```
##YAGO11k
python main.py -d YAGO11k -m TimePlex_base -a '{"embedding_dim":200, "srt_wt":5.0, "ort_wt":5.0, "sot_wt":0.0, "emb_reg_wt":0.03}' -l crossentropy_loss_AllNeg -r 0.1 -b 1500 -x 300 -n 0 -v 1 -q 0 -y 100 -g_reg 2 -g 1.0 --perturb_time 1 --mode train -e 100

##WIKIDATA12k
python main.py -d WIKIDATA12k -m TimePlex_base -a '{"embedding_dim":200, "srt_wt":5.0, "ort_wt":5.0, "sot_wt":5.0, "emb_reg_wt":0.005}' -l crossentropy_loss_AllNeg -r 0.1 -b 1500 -x 300 -n 0 -v 1 -q 0 -y 100 -g_reg 2 -g 2.0 --perturb_time 1 --mode train --flag_add_reverse 1 -e 100


##ICEWS05-15
python main.py -d icews05-15/large -m TimePlex_base -a '{"embedding_dim":200, "srt_wt": 5.0, "ort_wt": 5.0, "sot_wt": 5.0, "time_reg_wt":5.0, "emb_reg_wt":0.005}' -l crossentropy_loss_AllNeg -r 0.1 -b 1000 -x 2000 -n 0 -v 1 -q 0 -y 500 -g_reg 2 -g 1.0 --filter_method time-str -e 250 --flag_add_reverse 1 

##ICEWS14
CUDA_VISIBLE_DEVICES=5 python main.py -d icews14/large -m TimePlex_base -a '{"embedding_dim":200, "srt_wt": 5.0, "ort_wt": 5.0, "sot_wt": 5.0, "time_reg_wt":0.01, "emb_reg_wt":0.005}' -l crossentropy_loss_AllNeg -r 0.1 -b 1000 -x 2000 -n 0 -v 1 -q 0 -y 500 -g_reg 2 -g 1.0 --filter_method time-str -e 250 --flag_add_reverse 1 -z 1
```

## TimePlex-

Once the base model has been trained, we can augment it with either pair/recurrent features.
To train with pair features- 
```
python main.py -d YAGO11k -m TimePlex -a '{"embedding_dim":200, "model_path":"./models/yago_timeplex_base/best_valid_model.pt", "pairs_wt":3.0, , "pairs_args":{"reg_wt":0.002}}' -l crossentropy_loss -r 0.05 -b 100 -x 300 -n 100 -v 1 -q 0 -y 40  -g 1.0 -bt 0 --patience 1 -e 2
```

To train with recurrent features-

```
##YAGO11k-
python  main.py -d YAGO11k -m TimePlex -a '{"embedding_dim":200, "model_path":"./models/yago_timeplex_base/best_valid_model.pt", "recurrent_wt":5.0}' -l crossentropy_loss -r 1.0 -b 100 -x 600 -n 100 -v 1 -q 0 -y 40 -g_reg 2 -g 0.0 -bt 0 --patience 1 -e 10

##WIKIDATA12k-
python main.py -d WIKIDATA12k -m TimePlex -a '{"embedding_dim":200, "model_path":"./models/wiki_timeplex_base/best_valid_model.pt", "recurrent_wt":5.0}' -l crossentropy_loss -r 0.1 -b 100 -x 300 -n 100 -v 1 -q 0 -y 40 -g_reg 2 -g 0.0 -bt 0 --patience 1 -e 2
```

# Evaluating trained models(for link and time prediction)-

(Note: Replace `-m TimePlex` with `-m TimePlex_base` to evaluate TimePlex_base models)

For interval datasets-
```
## YAGO11k- 
python main.py -d YAGO11k -m TimePlex --resume_from_save "./models/yago_timeplex/best_valid_model.pt"  --mode test --predict_time 1 -y 40

## WIKIDATA12k- 
python main.py -d WIKIDATA12k -m TimePlex --resume_from_save "./models/wiki_timeplex/best_valid_model.pt"  --mode test --predict_time 1 -y 40
```

For event datasets-
```
## ICEWS05-15
python main.py -d icews05-15/large -m TimePlex --resume_from_save "./models/icews05-15_timeplex/best_valid_model.pt"  --mode test --filter-method time-str -y 40

## ICEWS14
python main.py -d icews05-15/large -m TimePlex --resume_from_save "./models/icews05-15_timeplex/best_valid_model.pt"  --mode test --filter-method time-str -y 40

```





<!-- ### These trained gadgets can be combined with the following command -
```
##YAGO11k

python command for yago
``` -->




