# Stable Baselines3

## Install
`poetry add 'stable-baselines3[extra]'`
```
  • Installing gym (0.21.0): Failed

  ChefBuildError

  Backend subprocess exited when trying to invoke get_requires_for_build_wheel

  error in gym setup command: 'extras_require' must be a dictionary whose values are strings or lists of strings containing valid project/version requirement specifiers.
```
```
Note: This error originates from the build backend, and is likely not a problem with poetry but with gym (0.21.0) not supporting PEP 517 builds. You can verify this by running 'pip wheel --use-pep517 "gym (==0.21.0)"'.
```

`pip3 wheel --use-pep517 "gym (==0.21.0)"`
```
Getting requirements to build wheel ... error
  ERROR: Command errored out with exit status 1:
   command: /usr/bin/python3 /tmp/tmpla7ly4s9 get_requires_for_build_wheel /tmp/tmpuj5nvklz
       cwd: /tmp/pip-wheel-ij9aco4e/gym
  Complete output (1 lines):
  error in gym setup command: 'extras_require' must be a dictionary whose values are strings or lists of strings containing valid project/version requirement specifiers.
  ----------------------------------------
ERROR: Command errored out with exit status 1: /usr/bin/python3 /tmp/tmpla7ly4s9 get_requires_for_build_wheel /tmp/tmpuj5nvklz Check the logs for full command output.
```
- PEP (Python environment proposal): Pythonにおける標準を定める
  - PEP 517: `pyproject.toml`において，インストール作業において実行するツールを指定すること

つまり，poetry のエラーではない．

`pip3 install --upgrade setuptools`による解決
- setuptools: pipの動作に必要なライブラリ．Pythonプロジェクトのパッケージ開発プロセスライブラリ．
```
ERROR: launchpadlib 1.10.13 requires testresources, which is not installed.
```

`sudo apt install python3-testresources`

解決しなかった

そもそもの問題: SB3に要求されるgymのバージョン(0.21.0)が古すぎてPEP517を満たしていない．

poetry側ではどうしようもないので，
`poetry run python3 -m pip install --no-use-pep517 gym==0.21.0`
をして，インストールするしかなさそう
```
  × python setup.py egg_info did not run successfully.
  │ exit code: 1
  ╰─> [1 lines of output]
      error in gym setup command: 'extras_require' must be a dictionary whose values are strings or lists of strings containing valid project/version requirement specifiers.
      [end of output]
```
`poetry run python3 -m pip install --upgrade setuptools`しても変わらず

ココにすべて書いてある
https://github.com/openai/gym/issues/3176

`poetry run python3 -m pip install setuptools==65.5.0`
をした後に，poetry add すれば多分治った．

## Pyglet
`NameError: name 'glPushMatrix' is not defined`
- use pyglet==1.5.27

```
ImportError:
    Error occurred while running `from pyglet.gl import *`
    HINT: make sure you have OpenGL installed. On Ubuntu, you can run 'apt-get install python-opengl'.
    If you're running on a server, you may need a virtual frame buffer; something like this should work:
    'xvfb-run -s "-screen 0 1400x900x24" python <your_script.py>'
```
`sudo apt install python3-opengl`
