# Dependency Management in Python (Part 1)

When you start a new Python project, it is recommended to work inside a 'Virtual Environment' Why?
Because your operating system itself would have some Python packages for its own sake. You know? To keep your computer running as it should.

For instance `/var/lib/dist.py` in my laptop running Ubuntu is a Python package in itself. Let's say it is at version 2.20. This is the version the Ubuntu project would have thoroughly tested before releasing it to my machine. But when I run `pip install dist` today I could get a newer version 2.23 which could mess up my machine. Alternatively, if I were to do a `pip install someOtherProject`, that `SomeOtherProject` could have for it to function as intended brought in and installed the `dist-2.23`. Now I can't be expected to be aware of all the Python packages used by Ubuntu that can conflict with some other project I am going to use over the days in my project. Am I?

Alternatively, think of yourself doing 2 different Python projects, where *Project A* requires *Dependency 1* of version 1.0.0 but *Project B* requires *Dependency 1* of version 1.1.1. On your machine, packages are saved with just the name. No version number.

The solution to this problem is to create a virtual environment for each of your projects. A virtual environment is a self-contained directory that contains a Python installation for a particular version of Python, plus several additional packages. This allows you to have different versions of the same package in different projects without any conflicts.
A virtual environment is created using the `venv` module, which is included in the Python standard library. To create a virtual environment, you can use the following command:

```bash
python3 -m venv myenv
```
This will create a new directory called `myenv` in your current working directory. The directory will contain a copy of the Python interpreter and a `lib` directory that contains the standard library and any additional packages you install.
To activate the virtual environment, you can use the following command:

```bash
source myenv/bin/activate
```
This will change your shell prompt to indicate that you are now working inside the virtual environment. You can now install packages using `pip`, and they will be installed in the `myenv` directory instead of the system-wide Python installation.
To deactivate the virtual environment, you can use the following command:

```bash
deactivate
```
This will return you to your system-wide Python installation.

## Installing Packages inside virtual environment
Once you have activated your virtual environment, you can install packages using `pip`. For example, to install the `requests` package, you can use the following command:

```bash
pip install requests
```
This will download and install the `requests` package and any dependencies it has. You can also specify a version of a package to install by using the `==` operator. For example, to install version 2.25.1 of the `requests` package, you can use the following command:

```bash
pip install requests==2.25.1
```
You can also install multiple packages at once by separating them with spaces. For example, to install both the `requests` and `numpy` packages, you can use the following command:

```bash
pip install requests numpy
```
