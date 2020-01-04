---
title: Saving a list of variables together into a compressed file
author: ''
date: '2019-10-06'
slug: saving-a-list-of-variables-together-into-a-compressed-file
categories: []
tags: []
---

In Python, there is no simple equivalent of R's `save` and `save.image` functions that would serialize a list of variables into a single compressed file. I have decided to change that.

```
import __main__
import pickle
from bz2 import BZ2File
from gzip import GzipFile
from typing import Optional, List


def save_env(path: str, objects: Optional[List[str]] = None, compress: str = 'gzip', protocol: int = -1):
    """
    Save the environment (list of objects) of Jupyter Notebook into a compressed file
    Args:
       path: path to the file
       objects: names of global objects to be saved
       compress: compression - default 'gzip', slower but better 'bz2', faster but much worse 'none'
       protocol: pickle protocol version (-1 = latest)
       
    Inspired by https://stackoverflow.com/questions/2960864/how-can-i-save-all-the-variables-in-the-current-python-session
    """
    if objects is None:
        # TODO(petr): if objects is None, save all objects
        raise NotImplementedError("Saving all objects not yet implemented.")

    with _get_file_handler(path, compress, "w") as fw:
        pickle.dump(objects, fw)
        for key in objects:
            try:
                pickle.dump(getattr(__main__, key), fw, protocol=protocol)
            except TypeError:
                raise TypeError('Don\'t know how to pickle: {}'.format(key))
            except AttributeError:
                raise AttributeError('Unknown object: {}'.format(key))


def load_env(path: str, compress: str = 'gzip') -> List[str]:
    """
    Load the environment (saved previously by `save_env`)
    Args:
       path: path to the file
       compress: compression used by `save_env`, either 'gzip' (default), 'bz2', or 'none'
    Returns:
        A list of object names that have been loaded.
    """

    with _get_file_handler(path, compress, "r") as fr:
        objects = pickle.load(fr)
        for key in objects:
            setattr(__main__, key, pickle.load(fr))

    return objects


def _get_file_handler(tmp_path, compress, mode):
    if compress == 'gzip':
        return GzipFile(tmp_path, mode)
    elif compress == 'bz2':
        return BZ2File(tmp_path, mode)
    elif compress == 'none':
        return open(tmp_path, mode + 'b')
    else:
        raise NotImplementedError('Compress method {} is not implemented. Use "gzip", "bz2" or "none" instead.'.format(compress))
```

**Original Gist**: https://gist.github.com/simecek/19588c61c3430834bec01c67effa9c4a