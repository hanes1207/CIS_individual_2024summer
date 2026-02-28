# Safety Gym (ESH version. edited by 24.07.11)
docker run -it --env DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix:ro ubuntu:20.04 /bin/bash

## Original work Information
- Official website: https://openai.com/research/safety-gym
- Code 1: https://github.com/openai/safety-gym
- Code 2: https://github.com/openai/safety-starter-agents
- Paper: https://cdn.openai.com/safexp-short.pdf
- DQN Code REF: https://github.com/PyTorchKorea/tutorials-kr/blob/master/intermediate_source/reinforcement_q_learning.py
- RAINBOW Code REF: https://github.com/Curt-Park/rainbow-is-all-you-need

<!-- ## Workstation2 CUDA Information
- GPU model name: NVIDIA GeForce RTX 4090 (simulator has not been tested on this model. `Simulator Requirements` in official documentation)
- GPU Cuda Compute Capability: 8.9 AdaLovelace (https://www.wikiwand.com/en/CUDA#/GPUs_supported)
- GPUs supported: CUDA SDK version 11.8, 12.0-12.3 (https://en.wikipedia.org/wiki/CUDA)
- NVIDIA driver installation: GeForce - GeForce RTX 40 Series - NVIDIA GeForce RTX 4090 - Linux 64bit (https://www.nvidia.com/Download/index.aspx?lang=kr)
- CUDA Toolkit download: CUDA Toolkit 12.2.2, Linux, x86_64, Ubuntu, 20.04, deb(local) (https://developer.nvidia.com/cuda-toolkit-archive)
- lib, include, bin update && path update for 3 (CUDA 12.2) -->


## Workstation2 Environment Changes (Original -> KKY)
- Python 3.6 or greater -> Python 3.10.13
- Ubuntu 16.04 -> Ubuntu 20.04
- mujoco_py==2.0.2.7 => mujoco-py<2.2,>=2.1
- numpy~=1.17.4 => numpy~=1.23.5
- (safety-starter-agent) tensorflow==1.13.1 => tensorflow[and-cuda]>=2.15.0 (https://www.tensorflow.org/install/source?hl=ko#tested_build_configurations)
- (safety-starter-agent) mpi4py==3.0.2 => mpi4py==3.1.4
- (dqn) pytorch==2.1.0 torchvision==0.16.0 torchaudio==2.1.0 pytorch-cuda=12.1 -c pytorch -c nvidia


## Setting & Run (for m2 macbook air)
0. Python setting
    - sudo apt-get update 
    - sudo apt-get install -y wget build-essential zlib1g-dev libncurses5-dev libgdbm-dev  libnss3-dev libssl-dev libreadline-dev libffi-dev curl libbz2-dev libsqlite3-dev
    - wget https://www.python.org/ftp/python/3.10.13/Python-3.10.13.tgz
    - tar -xf Python-3.10.13.tgz
    - cd Python-3.10.13
    - ./configure --enable-optimizations
    - make -j $(nproc)
    - sudo make altinstall
1. WSL setting
    - wsl.exe --install Ubuntu-18.04
    - wsl.exe --set-default Ubuntu-18.04
2. Conda installation + MUJOCO installation
    - wget https://repo.anaconda.com/archive/Anaconda3-2023.07-2-Linux-x86_64.sh
    => https://repo.anaconda.com/archive/Anaconda3-2024.06-1-Linux-aarch64.sh
    - export LC_ALL=C.UTF-8
    - export LANG=C.UTF-8
    - bash Anaconda3-2023.07-2-Linux-x86_64.sh
    - source ~/.bashrc
    - conda (to check)
    - wget https://mujoco.org/download/mujoco210-linux-x86_64.tar.gz
    => https://github.com/google-deepmind/mujoco/releases/download/3.1.6/mujoco-3.1.6-linux-aarch64.tar.gz
    - tar -zxvf mujoco-3.1.6-linux-x86_64.tar.gz
    - mkdir ~/.mujoco
    - cp -r mujoco210/ ~/.mujoco/mujoco210
    - sudo apt-get update
    - sudo apt-get install python3-pip gfortran pkg-config libopenblas-dev libosmesa6-dev patchelf libfreetype6-dev libopenmpi-dev
    - 
<!-- 1. CUDA Toolkit installation (after install driver in window11):
    - wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/cross-linux-aarch64/cuda-ubuntu2004.pin
    - sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
**    - wget https://developer.download.nvidia.com/compute/cuda/11.1.1/local_installers/cuda-repo-wsl-ubuntu-11-1-local_11.1.1-1_amd64.deb
**    - sudo dpkg -i cuda-repo-wsl-ubuntu-11-1-local_11.1.1-1_amd64.deb
    - sudo apt-key add /var/cuda-repo-wsl-ubuntu-11-1-local/7fa2af80.pub
    - sudo apt-get update
    - sudo apt-get -y install cuda
    - export PATH=/usr/local/cuda-11.1/bin:$PATH
    - export LD_LIBRARY_PATH=/usr/local/cuda-11.1/lib64:$LD_LIBRARY_PATH
    - nvidia-smi (checking)
    - nvcc --version (checking)
1. cuDNN installation:
    - Download file from https://developer.nvidia.com/downloads/compute/cudnn/secure/8.9.2/local_installers/11.x/cudnn-local-repo-ubuntu1804-8.9.2.26_1.0-1_amd64.deb/
    - sudo dpkg -i cudnn-local-repo-ubuntu1804-8.9.2.26_1.0-1_amd64.deb
    - sudo cp /var/cudnn-local-repo-ubuntu1804-8.9.2.26/cudnn-local-05B25A30-keyring.gpg /usr/share/keyrings/ -->
    - 
2. Package installation (safety-gym and safety-starter-agents)
    <!-- - Workstation1
        - conda create --name safegym python=3.8
        - conda activate safegym
        - pip install torch==1.9.1+cu111 torchvision==0.10.1+cu111 -f https://download.pytorch.org/whl/torch_stable.html
            - pip install torch==1.10.0+cu111 torchvision==0.11.0+cu111 -f https://download.pytorch.org/whl/torch_stable.html
        - git clone && cd CIS_23_safetygym
        - pip3 install -e .
        - cd safety-starter-agents
        - pip3 install 'cython<3'
        - pip3 install -e .
        - pip3 install gymnasium==0.26.3 matplotlib
        - cd ..
        - pip3 install sysv_ipc
        - conda install ignite -c pytorch -->
    - Workstation2
        - conda create --name safegym python=3.10.13
        - conda activate safegym
**        - conda install pytorch==2.1.0 torchvision==0.16.0-->0.16.1 torchaudio==2.1.0 pytorch-cuda=12.1 -c pytorch -c nvidia
**        - conda install cuda-toolkit
        - git clone && cd CIS_23_safetygym
        - pip3 install -e .
        - cd safety-starter-agents
        - pip3 install 'cython<3'
        - conda install fsspec
        - conda install -c conda-forge mpi4py=3.1.4 mpich
        - pip3 install -e .
        - pip3 install gymnasium==0.26.3 matplotlib sysv_ipc
        - conda install patchelf
        - conda install ignite -c pytorch
        - cd && vi./.bashrc
            - export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/root/.mujoco/mujoco210/bin
            - export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/nvidia
            - source ./.bashrc
1. MUJOCO checking
    - export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/root/.mujoco/mujoco-3.1.6/bin
    - Type below lines. If link(gcc) error occured during import mujoco_py:
        - sudo ln -s /usr/lib/x86_64-linux-gnu/libGL.so.1 /usr/lib/x86_64-linux-gnu/libGL.so

```
    python3
    >> import mujoco_py
    >> import os
    >> mj_path = mujoco_py.utils.discover_mujoco()
    >> xml_path = os.path.join(mj_path, 'model', 'humanoid.xml')
    >> model = mujoco_py.load_model_from_path(xml_path)
    >> sim = mujoco_py.MjSim(model)
    >> print(sim.data.qpos)
        [0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.]
    >> sim.step()
    >> print(sim.data.qpos)
        [-2.09531783e-19  2.72130735e-05  6.14480786e-22 -3.45474715e-06
        7.42993721e-06 -1.40711141e-04 -3.04253586e-04 -2.07559344e-04
        8.50646247e-05 -3.45474715e-06  7.42993721e-06 -1.40711141e-04
        -3.04253586e-04 -2.07559344e-04 -8.50646247e-05  1.11317030e-04
        -7.03465386e-05 -2.22862221e-05 -1.11317030e-04  7.03465386e-05
        -2.22862221e-05]
```
7. torch, GPU checking
    - cd CIS_23_safetygym
    - CUDA_VISIBLE_DEVICES=2 python ptcheck.py
8. Reproducing paper experiments
    - cd safety-starter-agents/scripts
    - python experiment.py --algo 'ppo' --task 'goal1' --robot 'point' --seed 0 --exp_name demo --cpu 1
    - python plot.py ../data/name of data
9. Running
    - Prepare executable file
        - cd RL_SYS
        - g++ grad.cpp -o grad.exe -lpthread
        - g++ gradkky.cpp -o gradkky.exe
        - sudo prlimit --stack=unlimited --pid $$; ulimit -s unlimited
    - `rainbow`
        - cd safety-starter-agents/scripts
        - python experiment.py --algo 'rainbow' --task 'goal1' --robot 'point' --seed 0 --exp_name rainbow --cpu 1
    - `dqn`
        - cd safety-starter-agents/scripts
        - python experiment.py --algo 'dqn' --task 'goal1' --robot 'point' --seed 0 --exp_name dqn --cpu 1
    - Comparison 0: benchmark - ppo, ppo_lagrangian, trpo
        - python plot.py ../data/ORG_ppo_PointGoal1 ../data/ORG_ppo_lagrangian_PointGoal1 ../data/ORG_trpo_PointGoal1 ../data/ORG_trpo_lagrangian_PointGoal1 ../data/ORG_cpo_PointGoal1
        - python plot.py ../data/ORG_ppo_PointGoal1 ../data/ORG_ppo_lagrangian_PointGoal1 ../data/ORG_trpo_PointGoal1 ../data/ORG_trpo_lagrangian_PointGoal1 ../data/ORG_cpo_PointGoal1 -y="AverageEpCost"
    - Comparison 1: dqn(org, reward), rainbow(org, reward). same observation.
        - python plot.py ../data/ORG_dqn_PointGoal1_reward ../data/ORG_rainbow_PointGoal1_reward
        - python plot.py ../data/ORG_dqn_PointGoal1_reward ../data/ORG_rainbow_PointGoal1_reward -y="AverageEpCost"
    - Comparison 2: ppo(org, reward, cost), dqn(org, reward, cost), rainbow(org, reward, cost). same observation.
        - python plot.py ../data/ORG_ppo_PointGoal1_reward+cost ../data/ORG_dqn_PointGoal1_reward+cost ../data/../data/ORG_rainbow_PointGoal1_reward+cost
        - python plot.py ../data/ORG_ppo_PointGoal1_reward+cost ../data/ORG_dqn_PointGoal1_reward+cost ../data/../data/ORG_rainbow_PointGoal1_reward+cost -y="AverageEpCost"
    - Comparison 3: ppo(org, reward, cost), rainbow(pof, reward, cost)
        - python plot.py ../data/ORG_ppo_PointGoal1_reward+cost ../data/POF_rainbow_PointGoal1_reward+cost
        - python plot.py ../data/ORG_ppo_PointGoal1_reward+cost ../data/POF_rainbow_PointGoal1_reward+cost -y="AverageEpCost"
    - 231204 dist comparison
        - python plot.py ../data/rainbow_trial1_dist ../data/ORG_rainbow_PointGoal1_reward
10. (Optional) Running tutorial
    - python safety_gym/random_agent.py
11. (Optional) check IPC things
    - ipcs -m: shared memory checking
        - ipcrm -m shmid: remove shm with shmid
    - ipcs -s: semaphore checking
        - ipcrm -s semid: remove semaphore with semid


## Remark
1. 23.09.19: SYSV IPC semaphore "ERANGE: Numerical result out of range" issue.
    - `semop` with SEM_UNDO flag for sem_wait and sem_post function cause error in fixed time.
    - semadj is per-process, and it will continuously increase in one process, and continuously decrease in the other.
    - temporal solution: `semctl` with SETVAL to clear semadj. (`sem_adj_reset` function)
2. 23.12.19: New workstation setting!
    - sudo ufw allow 8000/tcp
    - sudo vi /etc/ssh/sshd_config (port 8000)
    - 143.248.151.25 port 8000 (port forwarding to 192.168.1.229 port 8000)
    - CUDA_VISIBLE_DEVICES=2 python XX.py
3. 23.12.31: Move data from linux(server) to window(local)
    - scp -r -P 8000 kky@143.248.151.25:/home/kky/CIS_23_safetygym/safety-starter-agents/data/`dir name`/`data name`/ ./

## Additional References
- MUJOCO0: https://github.com/openai/mujoco-py
- MUJOCO1: https://github.com/ethz-asl/reinmav-gym/issues/35
- MUJOCO2: https://www.csestack.org/failed-building-wheel-pillow-python/
- MUJOCO3: https://github.com/openai/mujoco-py/issues/773
- MUJOCO4: https://github.com/openai/mujoco-py/issues/652
- SSH1: https://bebutae.tistory.com/198
- SSH2: https://pang2h.tistory.com/441
- VSCODE1: https://huilife.tistory.com/entry/VSCode-%EC%84%A4%EC%A0%95-3-CC-%EB%B9%8C%EB%93%9C-%EB%B0%8F-%EC%8B%A4%ED%96%89-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0
- MPI4PY1: https://github.com/mpi4py/mpi4py/releases
- TF1: https://coding-groot.tistory.com/87
- PYTHON CODE1(LAMBDA & MAP): https://tykimos.github.io/2020/01/01/Python_Lambda_Map/
- PYTHON CODE2(GATHER): https://data-newbie.tistory.com/709
- PYTHON CODE3(ZIP): https://scribblinganything.tistory.com/34
- PYTHON CODE4(SQ,UNSQ): https://sanghyu.tistory.com/86
- PYTHON CODE5(SUPER INIT): https://supermemi.tistory.com/entry/Python-3-super%ED%81%B4%EB%9E%98%EC%8A%A4-selfinit-%EC%97%90-%EB%8C%80%ED%95%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90
- PYTHON CODE6(GLOBAL & UNLOCAL): https://dojang.io/mod/page/view.php?id=2365
- DQN1: https://tutorials.pytorch.kr/intermediate/reinforcement_q_learning.html
- DQN2: https://ai-com.tistory.com/entry/RL-%EA%B0%95%ED%99%94%ED%95%99%EC%8A%B5-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-1-DQN-Deep-Q-Network
- DQN3: https://github.com/Curt-Park/rainbow-is-all-you-need/blob/master/08.rainbow.ipynb
- DQN4: https://velog.io/@lee9843/%EA%B3%A0%EB%A0%A4%EB%8C%80%ED%95%99%EA%B5%90-%EA%B0%95%ED%99%94%ED%95%99%EC%8A%B5%EC%98%A4%EC%8A%B9%EC%83%81%EA%B5%90%EC%88%98%EB%8B%98-1.-DRL-Introduction
- DQN5: https://www.youtube.com/watch?v=HXIbrL-glpU&list=PLvbUC2Zh5oJtYXow4jawpZJ2xBel6vGhC&index=1
- DQN6: https://www.slideshare.net/ssuserbd7730/pyconkr-2018-rladventureto-the-rainbow
- DQN7: https://velog.io/@everyman123/PRIORITIZED-EXPERIENCE-REPLAY-%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0
- DQN8: https://sumniya.tistory.com/3
- IPC1: https://stackoverflow.com/questions/35651059/sharing-information-between-a-python-code-and-c-code-ipc
- IPC-SHM1: https://github.com/dovanhuong/ipc_shared_memory_cpp_and_python/blob/main/shared_mem.py
- IPC-SHM2: https://reakwon.tistory.com/96
- IPC-SHM3: https://stackoverflow.com/questions/33226664/system-v-shared-memory-permission-bits-meaning-and-how-to-change
- IPC-SHM4: https://www.joinc.co.kr/w/man/2/shmat
- IPC-SHM5: https://dokhakdubini.tistory.com/490
- IPC-SEM1: https://blackinkgj.github.io/semaphore/
- IPC-SEM2: https://semanchuk.com/philip/
- IPC-SEM3: https://velog.io/@palinyee12/%EB%A6%AC%EB%88%85%EC%8A%A4-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-IPC-Semaphore-SYSV-POSIX
- IPC-SEM4: http://coffeenix.net/doc/develop/sema.txt
- IPC-SEM5: https://www.halolinux.us/communications/condition-flag-setaction-taken-by-semop-1.html
- IPC-SEM6: https://www.ibm.com/docs/en/zos/2.4.0?topic=functions-semop-semaphore-operations
- IPC-SEM7: https://man7.org/linux/man-pages/man2/semop.2.html
- IPC-SEM8: https://www.it-note.kr/106
- IPC-SEM9: https://manpages.ubuntu.com/manpages/trusty/man2/semctl.2.html
- EXCEPTION1: https://www.tutorialspoint.com/how-do-i-catch-a-ctrlplusc-event-in-cplusplus
- STACK1: https://stackoverflow.com/questions/2279052/increase-stack-size-in-linux-with-setrlimit/2279084#2279084
- GRADIENT ACCUMULATION: https://velog.io/@twinjuy/OOM%EB%A5%BC-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0-%EC%9C%84%ED%95%9C-Batch-Accumulation
- LR1: https://dacon.io/en/codeshare/2373
- LR2: https://pytorch.org/ignite/generated/ignite.handlers.param_scheduler.create_lr_scheduler_with_warmup.html
- LR3: https://pytorch.org/docs/stable/generated/torch.optim.lr_scheduler.LinearLR.html#torch.optim.lr_scheduler.LinearLR
- LR4: https://eagle705.github.io/Learning-rate-warmup-scheduling/

## Description from Original Code
### Short description 
To use the pre-configured environments from the Safety Gym benchmark suite, simply import the package and then use `gym.make`. For example:

```
import safety_gym
import gym

env = gym.make('Safexp-PointGoal1-v0')
```

For a complete list of pre-configured environments, see below.

To create a custom environment using the Safety Gym engine, use the `Engine` class. For example, to build an environment with a car robot, the push task, some hazards, and some vases, with constraints on entering the hazard areas but no constraints on hitting the vases:

```
from safety_gym.envs.engine import Engine

config = {
    'robot_base': 'xmls/car.xml',
    'task': 'push',
    'observe_goal_lidar': True,
    'observe_box_lidar': True,
    'observe_hazards': True,
    'observe_vases': True,
    'constrain_hazards': True,
    'lidar_max_dist': 3,
    'lidar_num_bins': 16,
    'hazards_num': 4,
    'vases_num': 4
}

env = Engine(config)
```

To register that custom environment with Gym:

```
from gym.envs.registration import register

register(id='SafexpTestEnvironment-v0',
         entry_point='safety_gym.envs.mujoco:Engine',
         kwargs={'config': config})
```

For a full list of configuration options, see the `Engine` [code itself](safety_gym/envs/engine.py). For a description of some common patterns and details that aren't obvious from the code, see the [section below](#using-engine-to-build-custom-environments).

The API for envs is the same as Gym:

```
next_observation, reward, done, info = env.step(action)
```

The `info` dict contains information about constraint costs. For example, in the custom environment we just built:

```
>>> info
{'cost_hazards': 0.0, 'cost': 0.0}
```
### Benchmark Suite
An environment in the Safety Gym benchmark suite is formed as a combination of a robot (one of `Point`, `Car`, or `Doggo`), a task (one of `Goal`, `Button`, or `Push`), and a level of difficulty (one of `0`, `1`, or `2`, with higher levels having more challenging constraints). Environments include:

* `Safexp-{Robot}Goal0-v0`: A robot must navigate to a goal.
* `Safexp-{Robot}Goal1-v0`: A robot must navigate to a goal while avoiding hazards. One vase is present in the scene, but the agent is not penalized for hitting it.
* `Safexp-{Robot}Goal2-v0`: A robot must navigate to a goal while avoiding more hazards and vases.
* `Safexp-{Robot}Button0-v0`: A robot must press a goal button.
* `Safexp-{Robot}Button1-v0`: A robot must press a goal button while avoiding hazards and gremlins, and while not pressing any of the wrong buttons. 
* `Safexp-{Robot}Button2-v0`: A robot must press a goal button while avoiding more hazards and gremlins, and while not pressing any of the wrong buttons.  
* `Safexp-{Robot}Push0-v0`: A robot must push a box to a goal.
* `Safexp-{Robot}Push1-v0`: A robot must push a box to a goal while avoiding hazards. One pillar is present in the scene, but the agent is not penalized for hitting it. 
* `Safexp-{Robot}Push2-v0`: A robot must push a box to a goal while avoiding more hazards and pillars.

(To make one of the above, make sure to substitute `{Robot}` for one of `Point`, `Car`, or `Doggo`.)
### Comparing Algorithms with Benchmark Scores
When using Safety Gym for research, we recommend comparing algorithms using aggregate metrics to represent performance across the entire benchmark suite or a subset of it. The aggregate metrics we recommend in the paper are:

* Average (over environments and random seeds) normalized average (over episodes) return of the final policy.
* Average normalized constraint violation of the final policy.
* Average normalized cost rate over training (sum of all costs incurred during training divided by number of environment interaction steps).

We compute normalized scores using reference statistics from our run of unconstrained PPO, with 10M env steps for environments with Point or Car robots and 100M env steps for environments with the Doggo robot. These reference statistics are available in [the bench folder](safety_gym/bench/characteristic_scores.json), and we provide a [utility function](safety_gym/bench/bench_utils.py#L40) to calculate normalized for an arbitrary environment.
### Using Engine to Build Custom Environments
Again, most of the conceptual details for Engine are described in the paper. But here, we'll describe some patterns and code details not covered there.

**Defaults for Sensors:** By default, the only sensors enabled are basic robot sensors: accelerometer, gyro, magnetometer, velocimeter, joint angles, and joint velocities. All other sensors (lidars for perceiving objects in the scene, vision, compasses, amount of time remaining, and a few others) are _disabled_ by default. To use them, you will have to explicitly enable them by passing in flags via the `Engine` config. Note that simply adding an object to a scene will not result in the corresponding sensor for that object becoming enabled, you have to pass the flag.

**Vision:** Vision is included as an option but is fairly minimally supported and we have not yet tested it extensively. Feature requests or bug-fixes related to vision will be considered low-priority relative to other functionality.

**Lidar and Pseudo-Lidar:** Lidar and pseudo-lidar are the main ways to observe objects. Lidar works by ray-tracing (using tools provided by MuJoCo), whereas pseudo-lidar works by looping over all objects in a scene, determining if they're in range, and then filling the appropriate lidar bins with the right values. They both share several details: in both cases, each lidar has a fixed number of bins spaced evenly around a full circle around the robot. 

Lidar-like observations are object-specific. That is, if you have hazards, vases, and goals in a scene, you would want to turn on the hazards lidar (through `observe_hazards`), the vases lidar (through `observe_vases`), and possibly the goals lidar (through `observe_goal_lidar`) as well. 

All lidar-like observations will be either true lidar or pseudo-lidar, depending on the `lidar_type` flag. By default, `lidar_type='pseudo'`. To use true lidar instead, set `lidar_type='natural'`.

Lidar observations are represented visually by "lidar halos" that hover above the agent. Each lidar halo has as many orbs as lidar bins, and an orb will light up if an object is in range of its corresponding bin. Lidar halos are nonphysical and do not interact with objects in the scene; they are purely there for the benefit of someone watching a video of the agent, so that it is clear what the agent is observing.

For pseudo-lidar specifically: normally, lidar-like observations would break the principle about small changes in state resulting in small changes in observation, since a small change in state could move an object from one bin to another.  We add a small “alias” signal for each bin into the neighboring bins, which smooths transitions between bins and additionally allows the observation to weakly localize an object within a bin.

**Defaults for Objects and Constraints:** By default, the only thing present in a scene is the robot (which defaults to `Car`). Everything else must be explicitly added. Adding an obstacle object (such as a hazard or a vase) to a scene does _not_ automatically add the constraint; if you want interactions with an obstacle to be constrained, you must also pass the flag to enable the constraint.

**Environment Layouts:** By default, environment layouts are randomly generated at the start of each episode. This behavior can be disabled by setting `randomize_layout=False`, in which case the environment layout is randomized once on initialization, and then it is reset to the same layout at the start of each new episode. Random layout generation works by sampling and can fail: the generator randomly places objects in a scene until there is a conflict (eg two objects overlap unacceptably). If it can't resolve the conflict by just resampling the last object placed, it throws the layout and starts over. If it can't find a valid layout after trying a (large) fixed number of times, `Engine` raises an exception. Details related to random object placement are described below.

**Placements, Locations, and Keepout:** For all of the different kinds of objects you can add to a Safety Gym environment, you can configure where they go in the scene through their `{object}s_placements`, `{object}s_locations`, and `{object}s_keepout` flags. You can set it up so that they are randomly placed around the scene at the start of each episode (through placements), or fixed to specific locations (through locations), and you can control how close they can be to other objects in the scene (through keepout).

`{object}s_placements` should be a list of (xmin, ymin, xmax, ymax) tuples, where each tuple describes a rectangular area where the object can be randomly placed. If none is given, it will default to the full size of the scene (given by the `placements_extents` flag). 

`{object}s_locations` should be a list of (x,y) locations where such objects should go exactly. 

At the start of an episode, when an environment layout is sampled, the layout sampler will first satisfy the `{object}s_locations` requirements. Suppose there are going to be 4 objects in the scene (specified with `{object}s_num`), and `{object}s_locations` is a list of 2 (x,y) locations. Then 2 objects will be placed on those locations. Afterwards, the remaining 2 objects will be randomly located according to `{object}s_placements`. If there are more locations than objects, the excess locations will be ignored.

`{object}s_keepout` specifies a radius around an object location that other objects are required to keep out of. Take caution in setting this: if objects and their keepouts are too big, and there are too many objects in the scene, the layout sampler may fail to generate a feasible layout.




## Citation of Paper
```
@article{Ray2019,
    author = {Ray, Alex and Achiam, Joshua and Amodei, Dario},
    title = {{Benchmarking Safe Exploration in Deep Reinforcement Learning}},
    year = {2019}
}
```


# Safety Starter Agents

A companion repo to the paper "Benchmarking Safe Exploration in Deep Reinforcement Learning," containing a variety of unconstrained and constrained RL algorithms.

This repo contains the implementations of PPO, TRPO, PPO-Lagrangian, TRPO-Lagrangian, and CPO used to obtain the results in the "Benchmarking Safe Exploration" paper, as well as experimental implementations of SAC and SAC-Lagrangian not used in the paper.

Note that the PPO implementations here follow the convention from [Spinning Up](https://spinningup.openai.com) rather than [Baselines](https://www.github.com/openai/baselines): they use the early stopping trick, omit observation and reward normalization, and do not use the clipped value loss, among other potential diffs. As a result, while it is easy to fairly compare this PPO to this TRPO, it is not the strongest PPO implementation (in the sense of sample efficiency) and can be improved on substantially.




## Getting Started

**Example Script:** To run PPO-Lagrangian on the `Safexp-PointGoal1-v0` environment from Safety Gym, using neural networks of size (64,64):

```
from safe_rl import ppo_lagrangian
import gym, safety_gym

ppo_lagrangian(
	env_fn = lambda : gym.make('Safexp-PointGoal1-v0'),
	ac_kwargs = dict(hidden_sizes=(64,64))
	)

```


**Reproduce Experiments from Paper:** To reproduce an experiment from the paper, run:

```
cd /path/to/safety-starter-agents/scripts
python experiment.py --algo ALGO --task TASK --robot ROBOT --seed SEED --exp_name EXP_NAME --cpu CPU
python experiment.py --algo 'ppo' --task 'goal1' --robot 'point' --seed 0 --exp_name demo --cpu 1
```

where 

* `ALGO` is in `['ppo', 'ppo_lagrangian', 'trpo', 'trpo_lagrangian', 'cpo']`.
* `TASK` is in `['goal1', 'goal2', 'button1', 'button2', 'push1', 'push2']` .
* `ROBOT` is in `['point', 'car', 'doggo']`.
* `SEED` is an integer. In the paper experiments, we used seeds of 0, 10, and 20, but results may not reproduce perfectly deterministically across machines.
* `CPU` is an integer for how many CPUs to parallelize across.

`EXP_NAME` is an optional argument for the name of the folder where results will be saved. The save folder will be placed in `/path/to/safety-starter-agents/data`. 


**Plot Results:** Plot results with:

```
cd /path/to/safety-starter-agents/scripts
python plot.py data/path/to/experiment
python plot.py
```

**Watch Trained Policies:** Test policies with:

```
cd /path/to/safety-starter-agents/scripts
python test_policy.py data/path/to/experiment
```
