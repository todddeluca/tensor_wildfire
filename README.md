# Tensor Wildfire

This is a pytorch rewrite of parts of my Evolutionary Computation final project with Max Green
and Karl Kaiser. So if you see references to final projects or EvoComp or CS352
or Fall 2019 don't panic. The initial code for this repo is copied from the 
final project repo:

https://github.com/maxfieldEland/EC_2019_FinalProject


## Experiment 1 Usage

Create a 20 landscape dataset, 10 for training and 10 for testing.

```
rm -rf dataset && python mapsynth.py make_landscapes --dataset-dir=dataset_exp1 -n20 -L100 --scale=400 --seed=1
time python mapsynth.py burn_landscapes --dataset-dir=dataset_exp1 --max-time=20 --num-periods=2 --seed=1 --func-type balanced_logits
```

Evolve the models, tracking their performance per evaluation 
and across generations, and evaluating the final evolved model 
on the training and test sets. (This might take a while.)

```
# the true model
time python nullexperiment.py run_experiment --dataset-dir=dataset_exp1 --out-dir=real_results/dataset_exp1 --model-type=balanced_logits --n-rep=16 --pop-size=100 --n-gen=50

# the constant null model
time python nullexperiment.py run_experiment --dataset-dir=dataset_exp1 --out-dir=real_results/dataset_exp1 --model-type=constant --n-rep=16 --pop-size=100 --n-gen=50 --mutpb=0.8 --cxpb=0.7

# the logistic null model
time python nullexperiment.py run_experiment --dataset-dir=dataset_exp1 --out-dir=real_results/dataset_exp1 --model-type=logistic --n-rep=16 --pop-size=100 --n-gen=50 --mutpb=1.0 --cxpb=0.4

# the genetic programming model
time python nullexperiment.py run_experiment --dataset-dir=dataset_exp1 --out-dir=real_results/dataset_exp1 --model-type=gp --n-rep=16 --pop-size=100 --n-gen=50 --mutpb=0.08 --cxpb=0.6
```

## Experiment 2 Usage

Create a single landscape represented by burn states in multiple timesteps: 
One landscape, with 10 saved periods, 8 timesteps in between each:
```
rm -rf dataset && python mapsynth.py make_landscapes --dataset-dir=dataset_exp2 -n1 -L100 --scale=400 --seed=1
time python mapsynth.py burn_landscapes --dataset-dir=dataset_exp2 --max-time=8 --num-periods=10 --seed=1 --func-type balanced_logits
```

Evolve the timestep-dependent model, tracking performances per evaluation
and across generations, and evaluating the final evolved model on the training
and testing sets.
```
# the true model
time python nullexperiment.py run_timesteps --dataset-dir=dataset_exp2 --out-dir=real_results/dataset_exp2 --model-type=balanced_logits --n-rep=16 --pop-size=100 --n-gen=50

# the constant null model
time python nullexperiment.py run_timesteps --dataset-dir=dataset_exp2 --out-dir=real_results/dataset_exp2 --model-type=constant --n-rep=16 --pop-size=100 --n-gen=50 --mutpb=0.8 --cxpb=0.7

# the logistic null model
time python nullexperiment.py run_timesteps --dataset-dir=dataset_exp2 --out-dir=real_results/dataset_exp2 --model-type=logistic --n-rep=16 --pop-size=100 --n-gen=50 --mutpb=1.0 --cxpb=0.4

# the genetic programming model
time python nullexperiment.py run_timesteps --dataset-dir=dataset_exp2 --out-dir=real_results/dataset_exp2 --model-type=gp --n-rep=16 --pop-size=100 --n-gen=50 --mutpb=0.08 --cxpb=0.6
```

## Test Set Variation Performance

Create 100 landscapes, 10 for training and 9 test sets of 10 landscapes each.

```
rm -rf dataset100 && python mapsynth.py make_landscapes --dataset-dir=dataset100 -n100 -L100 --scale=400 --seed=1
time python mapsynth.py burn_landscapes --dataset-dir=dataset100 --max-time=20 --num-periods=2 --seed=1 --func-type balanced_logits
```
 
Examine the variance of the test set fitness of a model by evaluating it on multiple test sets:

```
time python nullexperiment.py run_variance_experiment --dataset-dir=dataset100 --model-type=balanced_logits --batch-size=10 --out-dir=results_variance/dataset100
```
