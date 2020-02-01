# MidiMonster Wiki

### Initial Setup

```shell
$ virtualenv venv --python=python3.7
$ source venv/bin/activate
$ pip install -r requirements.txt
```

### Building

```shell
$ source venv/bin/activate
$ make html
```

The HTML views are now present in `build/html`.

To open the current build in `build/html` you can use:
```
python -m webbrowser "./build/html/index.html"
```


## License

All text and code in this repository is licensed under [ BSD-2-Clause][].
All project logos are property of the respective project.

[ BSD-2-Clause]: https://opensource.org/licenses/BSD-2-Clause
