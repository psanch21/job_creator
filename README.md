# Job Creator: Unleash the Power of HTCondor for ML Experiments 🚀

A repo to simplify bulk experiment submissions & manage Machine Learning jobs on HTCondor efficiently.

## Getting Started 🏁

These instructions will help you set up the job_creator toolkit on your local machine. This code has been tested on Python 3.6 or higher.


### Installation 🔧

1. Clone the repository:

```bash
git clone https://github.com/psanch21/job_creator.git
```


2. Change directory to the cloned repository:

```bash
cd job_creator
```


3. Install the required dependencies:

```bash
pip install -r requirements.txt
```


### Usage 📚

1. Tailor your experiment grid by configuring a `grid.yaml` file. An example in provided in the grids folder.

2. Configure your HTCondor submission parameters in a `cluster.yaml` file. Check out the grids folder for exemplary GPU and CPU configurations.

3. Run the `generate_jobs.py` script to generate experiment folders and submission files:

```bash
python generate_jobs.py --grid_file <GRID_YAML_FILE> --cluster_file <CLUSTER_YAML_FILE> --format <FORMAT> [...]
```
### Submission Example 1 🚀

In this example, we launch 10 configurations per job (`--batch_size 10`) and set a maximum of 200 jobs per submission file (`--jobs_per_file 200`).

```bash
python generate_jobs.py --grid_file grids/grid_1.yaml --cluster_file grids/cluster_cpu.yaml --format sub --jobs_per_file 200 --batch_size 10
```

This command creates three submission files, ready to be submitted to HTCondor using the `condor_submit` or `condor_submit_bid` commands:

```
condor_submit_bid 15 grids/grid_1/jobs_sub/jobs_1.sub
condor_submit_bid 15 grids/grid_1/jobs_sub/jobs_2.sub
condor_submit_bid 15 grids/grid_1/jobs_sub/jobs_3.sub
```


### Submission Example 2 🌟

In this example, we launch one configuration per job and add two extra arguments to each job: `--wandb_mode disabled` and `--project TEST`.


```bash
python generate_jobs.py --grid_file grids/grid_1.yaml --cluster_file grids/cluster_cpu.yaml --format sub --jobs_per_file 20000 --batch_size 1 --opts wandb_mode disabled project TEST 
```

Here's the beginning of the generated `grids/grid_1/jobs_sub/jobs_1.sub` file:

```
executable = /home/psanch21/miniconda3/envs/my_env/bin/python
request_memory = 6000
request_cpus = 1
getenv = true


arguments = "main.py --config_file grids/grid_1/configs/1/config_1.yaml --wandb_mode disabled --project TEST"
error = grids/grid_1/output_sub/1/job_0.err
output = grids/grid_1/output_sub/1/job_0.out
log = grids/grid_1/output_sub/1/job_0.log
queue

[...]
```

### Submission Example 3 🔥
In this example, we launch one configuration per job and add two extra arguments to each job: `--wandb_mode disabled` and `--project TEST`. Now, we generate a shell file instead of a Condor submission file, thanks to `--format shell`.



```bash
python generate_jobs.py --grid_file grids/grid_1.yaml --cluster_file grids/cluster_cpu.yaml --format shell --jobs_per_file 20000 --batch_size 1 --opts wandb_mode disabled project TEST 
```

This is the start of the generated `grids/grid_1/jobs_sh/jobs_1.sh` file:

```bash

python main.py --config_file grids/grid_1/configs/1/config_1.yaml --wandb_mode disabled --project TEST

python main.py --config_file grids/grid_1/configs/1/config_2.yaml --wandb_mode disabled --project TEST

python main.py --config_file grids/grid_1/configs/1/config_3.yaml --wandb_mode disabled --project TEST

python main.py --config_file grids/grid_1/configs/1/config_4.yaml --wandb_mode disabled --project TEST

python main.py --config_file grids/grid_1/configs/1/config_5.yaml --wandb_mode disabled --project TEST

[...]
```

### Extra Arguments 🛠️

You can add extra arguments to each job using the `--opts` option when running the `generate_jobs.py`. The format is simple: `--opts ARG_NAME_1 VALUE_1 ARG_NAME_2 VALUE_2`. For example, if you want to include additional parameters:

```bash
python generate_jobs.py --grid_file grids/grid_1.yaml --cluster_file grids/cluster_cpu.yaml --format sub --jobs_per_file 20000 --batch_size 1 --opts plot True only_batch_id 3 
```

This command adds the `--plot True` and `--only_batch_id 3` arguments to each job. Customize your job submissions with ease using the `--opts` option!

## Contributing 🤝

We welcome contributions from the community! If you'd like to contribute to the Job Creator project, follow these steps:

- Fork the repository.
- Create a new branch for your feature or bugfix.
- Commit your changes to the new branch.
- Submit a pull request to the main repository, describing your changes and their purpose.

We will review your pull request, provide feedback, and merge your changes once they meet the project requirements.



## License 📄

This project is licensed under the MIT License - see the [LICENSE](https://github.com/psanch21/job_creator/blob/main/LICENSE) file for details.

## Contact 📬
If you have any questions or suggestions, feel free to reach out to us by opening an issue on GitHub or contacting us via [email](psanch2103@gmail.com).
