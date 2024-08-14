# CIS_individual_2024summer
KAIST CIS LAB individual study 2024 summer
For a fool people who bought MacBook...

August 14, 2024 Euiseok Han
Laptop: MacBook Air 15(M2, 2023)

**safety-gym**
# version update and print goal commit
engine.py
    - print "GOAL!" whenever met goal
setup.py
    - match version: mujoco-py numpy tensorflow mpi4py

**safety-starter-agents**
# version update and tf compatibility btw tf.v1 and tf.v2
setup.py
    - match version: mpi4py mujoco-py numpy tensorflow
network.py
    - tf compatibility btw tf.v1 and tf.v2
        import tensorflow as tf --> import tensorflow.compat.v1 as tf
        ++ tf.disable_eager_execution()
run_agent.py, trust_region.py, sac.py, load_utils.py, logx.py, mpi_tf.py
    - tf compatibility btw tf.v1 and tf.v2
plot.py
    - ++ import pdb # for debugging
comparision.png
    - result sample