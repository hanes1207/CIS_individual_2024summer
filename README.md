# CIS_individual_2024summer
KAIST CIS LAB individual study 2024 summer

For a fool people who bought MacBook...

August 14, 2024 Euiseok Han

Laptop: MacBook Air 15(M2, 2023)

# safety-gym
**version update and print goal commit**
- engine.py - print "GOAL!" whenever met goal
- setup.py
  - match version: mujoco-py numpy tensorflow mpi4py

# safety-starter-agents
**version update and tf compatibility btw tf.v1 and tf.v2**
- setup.py
  - match version: mpi4py mujoco-py numpy tensorflow
- network.py
  - tf compatibility btw tf.v1 and tf.v2
    - import tensorflow as tf --> import tensorflow.compat.v1 as tf
    - ++ tf.disable_eager_execution()
- run_agent.py, trust_region.py, sac.py, load_utils.py, logx.py, mpi_tf.py
  - tf compatibility btw tf.v1 and tf.v2
- plot.py
  - ++ import pdb # for debugging
  - matplotlib no longer has tsplot function: sns.tsplot --> sns.lineplot
  - set ylim: plt.ylim(bottom=0) / plt.ylim(top=30)
- comparision.png
  - result sample

# How to run download mujoco?
- Reference sites
  - <https://ropiens.tistory.com/178?category=1027022>
  - <https://github.com/openai/mujoco-py/issues/662>
  - <https://github.com/google-deepmind/mujoco/releases/tag/2.1.1>

**본인은 https://github.com/google-deepmind/mujoco/releases/download/2.1.1/mujoco-2.1.1-macos-universal2.dmg 파일을 ~/Volumns/MuJoCo안의 파일들을 ~/mujoco/안에 넣어놓음**


1. Make Virtual Environment
- use mini-forge, not anaconda: brew install mini-forge
- conda create -n safetym python=3.9.19
- when deactivating: conda deactivate
2. Download mujoco engine
- mkdir -p $HOME/.mujoco/mujoco210
- cd ~/.mujoco/mujoco210
- mkdir bin include
- cd ~/.mujoco/mujoco210/bin
- cp -r ~/mujoco/MuJoCo.framework/Versions/Current/libmujoco.2.1.1.dylib libmujoco210.dylib
- cd ~/.mujoco/mujoco210/include
- cp -r ~/mujoco/MuJoCo.framework/Versions/Current/Headers/ .
- brew install glfw
- cd ~/.mujoco/mujoco210/bin
- ln -s /opt/homebrew/lib/libglfw.3.dylib .
- export CC=/opt/homebrew/bin/gcc-11
3. install mujoco_py, safety-gym, safety-starter-agent
- git clone https://github.com/openai/safety-gym
- git clone https://github.com/openai/safety-starter-agents
- cd ~/safety-gym
- pip3 install -e .
- cd ~/safety-starter-agents
- pip3 install -e .
4. import mujoco_py
- python
- import mujoco_py
### Troble shooting 1

```
(safegym) hanes1207@EuiseokHan-MacbookAir safety-starter-agents % python
Python 3.9.19 | packaged by conda-forge | (main, Mar 20 2024, 12:55:20) 
[Clang 16.0.6 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import mujoco_py
Compiling /opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/cymj.pyx because it changed.
[1/1] Cythonizing /opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/cymj.pyx
performance hint: /opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/cymj.pyx:67:5: Exception check on 'c_warning_callback' will always require the GIL to be acquired.
Possible solutions:
        1. Declare 'c_warning_callback' as 'noexcept' if you control the definition and you're sure you don't want the function to raise exceptions.
        2. Use an 'int' return type on 'c_warning_callback' to allow an error code to be returned.
performance hint: /opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/cymj.pyx:104:5: Exception check on 'c_error_callback' will always require the GIL to be acquired.
Possible solutions:
        1. Declare 'c_error_callback' as 'noexcept' if you control the definition and you're sure you don't want the function to raise exceptions.
        2. Use an 'int' return type on 'c_error_callback' to allow an error code to be returned.

Error compiling Cython file:
------------------------------------------------------------
...
    See c_warning_callback, which is the C wrapper to the user defined function
    '''
    global py_warning_callback
    global mju_user_warning
    py_warning_callback = warn
    mju_user_warning = c_warning_callback
                       ^
------------------------------------------------------------

/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/cymj.pyx:92:23: Cannot assign type 'void (const char *) except * nogil' to 'void (*)(const char *) noexcept nogil'. Exception values are incompatible. Suggest adding 'noexcept' to the type of 'c_warning_callback'.

Error compiling Cython file:
------------------------------------------------------------
...
    See c_warning_callback, which is the C wrapper to the user defined function
    '''
    global py_error_callback
    global mju_user_error
    py_error_callback = err_callback
    mju_user_error = c_error_callback
                     ^
------------------------------------------------------------

/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/cymj.pyx:127:21: Cannot assign type 'void (const char *) except * nogil' to 'void (*)(const char *) noexcept nogil'. Exception values are incompatible. Suggest adding 'noexcept' to the type of 'c_error_callback'.
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/__init__.py", line 2, in <module>
    from mujoco_py.builder import cymj, ignore_mujoco_warnings, functions, MujocoException
  File "/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/builder.py", line 504, in <module>
    cymj = load_cython_ext(mujoco_path)
  File "/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/builder.py", line 110, in load_cython_ext
    cext_so_path = builder.build()
  File "/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/builder.py", line 226, in build
    built_so_file_path = self._build_impl()
  File "/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/builder.py", line 343, in _build_impl
    so_file_path = super()._build_impl()
  File "/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/builder.py", line 239, in _build_impl
    dist.ext_modules = cythonize([self.extension])
  File "/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/Cython/Build/Dependencies.py", line 1154, in cythonize
    cythonize_one(*args)
  File "/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/Cython/Build/Dependencies.py", line 1321, in cythonize_one
    raise CompileError(None, pyx_file)
Cython.Compiler.Errors.CompileError: /opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/cymj.pyx
```
- pip install 'cython<3'
- python
- import mujoco_py

### Troble Shooting 2
```
(safegym) hanes1207@EuiseokHan-MacbookAir safety-starter-agents % python
Python 3.9.19 | packaged by conda-forge | (main, Mar 20 2024, 12:55:20) 
[Clang 16.0.6 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import mujoco_py
Compiling /opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/cymj.pyx because it changed.
[1/1] Cythonizing /opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/cymj.pyx
ld: warning: dylib (/opt/homebrew/Cellar/gcc@11/11.4.0/lib/gcc/11/libgomp.dylib) was built for newer macOS version (14.0) than being linked (11.0)
ld: warning: dylib (/Users/hanes1207/.mujoco/mujoco210/bin/libglfw.3.dylib) was built for newer macOS version (14.0) than being linked (11.0)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/__init__.py", line 2, in <module>
    from mujoco_py.builder import cymj, ignore_mujoco_warnings, functions, MujocoException
  File "/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/builder.py", line 504, in <module>
    cymj = load_cython_ext(mujoco_path)
  File "/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/builder.py", line 111, in load_cython_ext
    mod = load_dynamic_ext('cymj', cext_so_path)
  File "/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/builder.py", line 130, in load_dynamic_ext
    return loader.load_module()
ImportError: dlopen(/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/generated/cymj_2.1.2.14_39_macextensionbuilder_39.so, 0x0002): Library not loaded: @rpath/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib
  Referenced from: <98B601EE-8E50-32AF-924D-D6F6A7B2A8AE> /opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/generated/cymj_2.1.2.14_39_macextensionbuilder_39.so
  Reason: tried: '/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/Users/hanes1207/.mujoco/mujoco210/bin/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/Users/hanes1207/.mujoco/mujoco210/bin/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/generated/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/opt/homebrew/Cellar/gcc@11/11.4.0/lib/gcc/11/gcc/aarch64-apple-darwin23/11/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/opt/homebrew/Cellar/gcc@11/11.4.0/lib/gcc/11/gcc/aarch64-apple-darwin23/11/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/opt/homebrew/Cellar/gcc@11/11.4.0/lib/gcc/11/gcc/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/opt/homebrew/Cellar/gcc@11/11.4.0/lib/gcc/11/gcc/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/opt/homebrew/Cellar/gcc@11/11.4.0/lib/gcc/11/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/opt/homebrew/Cellar/gcc@11/11.4.0/lib/gcc/11/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/Users/hanes1207/.mujoco/mujoco210/bin/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/Users/hanes1207/.mujoco/mujoco210/bin/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/opt/homebrew/Caskroom/miniforge/base/envs/safegym/lib/python3.9/site-packages/mujoco_py/generated/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/opt/homebrew/Cellar/gcc@11/11.4.0/lib/gcc/11/gcc/aarch64-apple-darwin23/11/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/opt/homebrew/Cellar/gcc@11/11.4.0/lib/gcc/11/gcc/aarch64-apple-darwin23/11/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/opt/homebrew/Cellar/gcc@11/11.4.0/lib/gcc/11/gcc/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/opt/homebrew/Cellar/gcc@11/11.4.0/lib/gcc/11/gcc/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/opt/homebrew/Cellar/gcc@11/11.4.0/lib/gcc/11/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/opt/homebrew/Cellar/gcc@11/11.4.0/lib/gcc/11/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/opt/homebrew/Caskroom/miniforge/base/envs/safegym/bin/../lib/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file), '/opt/homebrew/Caskroom/miniforge/base/envs/safegym/bin/../lib/MuJoCo.framework/Versions/A/libmujoco.2.1.1.dylib' (no such file)
```
export DYLD_LIBRARY_PATH=$DYLD_LIBRARY_PATH:~/mujoco/MuJoCo.app/Contents/Frameworks/MuJoCo.framework/Versions/Current

it works!

5. Plotting graph
- python plot.py ../data
### Troble Shooting 3
```
(safegym) hanes1207@EuiseokHan-MacbookAir scripts % python plot.py ../data
/Users/hanes1207/safety-starter-agents/scripts/plot.py:135: SyntaxWarning: "is not" with a literal. Did you mean "!="?
  if savedir is not '':
/Users/hanes1207/safety-starter-agents/scripts/plot.py:142: SyntaxWarning: "is not" with a literal. Did you mean "!="?
  if savedir is not '':
/Users/hanes1207/safety-starter-agents/scripts/plot.py:145: SyntaxWarning: "is not" with a literal. Did you mean "!="?
  if savedir is not '':
Plotting from...
==================================================

../data

==================================================
Could not read from ../data/2024-08-14_ppo_PointGoal1/2024-08-14_19-19-42-ppo_PointGoal1_s0/progress.txt
Traceback (most recent call last):
  File "/Users/hanes1207/safety-starter-agents/scripts/plot.py", line 347, in <module>
    main()
  File "/Users/hanes1207/safety-starter-agents/scripts/plot.py", line 340, in main
    make_plots(args.logdir, args.legend, args.xaxis, args.value, args.count, 
  File "/Users/hanes1207/safety-starter-agents/scripts/plot.py", line 263, in make_plots
    plot_data(data, xaxis=xaxis, value=value, condition=condition, 
  File "/Users/hanes1207/safety-starter-agents/scripts/plot.py", line 77, in plot_data
    sns.lineplot(data=data, x=xaxis, y=value, hue=condition, ci='sd', **kwargs)
AttributeError: module 'seaborn' has no attribute 'lineplot'
```
- pip uninstall matplotlib
- pip install matplotlib==3.7.3
- python plot.py ../data

### Troble Shooting 4
```
(safegym) hanes1207@EuiseokHan-MacbookAir scripts % python plot.py ../data       
/Users/hanes1207/safety-starter-agents/scripts/plot.py:135: SyntaxWarning: "is not" with a literal. Did you mean "!="?
  if savedir is not '':
/Users/hanes1207/safety-starter-agents/scripts/plot.py:142: SyntaxWarning: "is not" with a literal. Did you mean "!="?
  if savedir is not '':
/Users/hanes1207/safety-starter-agents/scripts/plot.py:145: SyntaxWarning: "is not" with a literal. Did you mean "!="?
  if savedir is not '':
Plotting from...
==================================================

../data

==================================================
Could not read from ../data/2024-08-14_ppo_PointGoal1/2024-08-14_19-19-42-ppo_PointGoal1_s0/progress.txt
Traceback (most recent call last):
  File "/Users/hanes1207/safety-starter-agents/scripts/plot.py", line 347, in <module>
    main()
  File "/Users/hanes1207/safety-starter-agents/scripts/plot.py", line 340, in main
    make_plots(args.logdir, args.legend, args.xaxis, args.value, args.count, 
  File "/Users/hanes1207/safety-starter-agents/scripts/plot.py", line 263, in make_plots
    plot_data(data, xaxis=xaxis, value=value, condition=condition, 
  File "/Users/hanes1207/safety-starter-agents/scripts/plot.py", line 77, in plot_data
    sns.lineplot(data=data, x=xaxis, y=value, hue=condition, ci='sd', **kwargs)
AttributeError: module 'seaborn' has no attribute 'lineplot'
```
- pip uninstall seaborn
- pip install seaborn==0.9.0

# Finally Done!

# How to simulate?
- cd safety-starter-agents/scripts
- python experiment.py --algo 'ppo' --task 'goal1' --robot 'point' --seed 0 --exp_name demo --cpu 1
- python plot.py ../data/name of data

## Possible Troble... (That were met but not in the above because I didn't remember...)

### pip wheel error
- pip install cffi
- pip install mpi4py==3.1.4

### lib error
- there should be libglfw.3.dylib in ~/.mujoco/mujoco210/bin
- there should be libmujoco210.dylib in ~/.mujoco/mujoco210/bin/

### file name error
- ~/.mujoco/mujoco210/, not ~/.mujoco/mujoco-2.1.1/

### header file error
- *.h in ~/.mujoco/mujoco210/include/, not ~/.mujoco/mujoco210/include/header/