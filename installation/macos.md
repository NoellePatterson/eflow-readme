## MacOS Installation

1. Install [Python3](https://www.python.org/downloads/), [Git](https://git-scm.com/download/) and a [text editor](https://www.sublimetext.com/3) of your choice.

2. Clone the FFC directory as a project in [Terminal](http://www.informit.com/blogs/blog.aspx?uk=The-10-Most-Important-Linux-Commands)

   ```
   git clone https://github.com/leogoesger/func-flow.git
   cd func-flow/
   ```

3. Create and activate a [virtual environment](https://docs.python.org/3/library/venv.html).

   ```
   python3 -m venv my-virtualenv
   source my-virtualenv/bin/activate
   ```

4. Install dependencies using pip.

   ```
   pip install -r requirements.txt
   ```
