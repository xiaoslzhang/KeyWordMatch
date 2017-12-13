.. FlashText documentation master file, created by
   sphinx-quickstart on Wed Aug 16 22:47:29 2017.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


FlashText's documentation!
==========================

.. image:: https://api.travis-ci.org/vi3k6i5/flashtext.svg?branch=master
    :target: https://travis-ci.org/vi3k6i5/flashtext
    :alt: Build Status

.. image:: https://readthedocs.org/projects/flashtext/badge/?version=latest
    :target: http://flashtext.readthedocs.io/en/latest/?badge=latest
    :alt: Documentation Status

.. image:: https://badge.fury.io/py/flashtext.svg
    :target: https://badge.fury.io/py/flashtext
    :alt: Version

.. image:: https://coveralls.io/repos/github/vi3k6i5/flashtext/badge.svg?branch=master
   :target: https://coveralls.io/github/vi3k6i5/flashtext?branch=master
   :alt: Test coverage

.. image:: https://img.shields.io/github/license/mashape/apistatus.svg?maxAge=2592000
    :target: https://github.com/vi3k6i5/flashtext/blob/master/LICENSE
    :alt: license

This module can be used to replace keywords in sentences or extract keywords from sentences. It is based on the `FlashText algorithm <https://arxiv.org/abs/1711.00046>`_.


Installation
------------
::

    $ pip install flashtext


Usage
-----
Extract keywords
    >>> from flashtext import KeywordProcessor
    >>> keyword_processor = KeywordProcessor()
    >>> # keyword_processor.add_keyword(<unclean name>, <standardised name>)
    >>> keyword_processor.add_keyword('Big Apple', 'New York')
    >>> keyword_processor.add_keyword('Bay Area')
    >>> keywords_found = keyword_processor.extract_keywords('I love Big Apple and Bay Area.')
    >>> keywords_found
    >>> # ['New York', 'Bay Area']

Replace keywords
    >>> keyword_processor.add_keyword('New Delhi', 'NCR region')
    >>> new_sentence = keyword_processor.replace_keywords('I love Big Apple and new delhi.')
    >>> new_sentence
    >>> # 'I love New York and NCR region.'

Case Sensitive example
    >>> from flashtext import KeywordProcessor
    >>> keyword_processor = KeywordProcessor(case_sensitive=True)
    >>> keyword_processor.add_keyword('Big Apple', 'New York')
    >>> keyword_processor.add_keyword('Bay Area')
    >>> keywords_found = keyword_processor.extract_keywords('I love big Apple and Bay Area.')
    >>> keywords_found
    >>> # ['Bay Area']

No clean name for Keywords
    >>> from flashtext import KeywordProcessor
    >>> keyword_processor = KeywordProcessor()
    >>> keyword_processor.add_keyword('Big Apple')
    >>> keyword_processor.add_keyword('Bay Area')
    >>> keywords_found = keyword_processor.extract_keywords('I love big Apple and Bay Area.')
    >>> keywords_found
    >>> # ['Big Apple', 'Bay Area']

Add Multiple Keywords simultaneously
    >>> from flashtext import KeywordProcessor
    >>> keyword_processor = KeywordProcessor()
    >>> keyword_dict = {
    >>>     "java": ["java_2e", "java programing"],
    >>>     "product management": ["PM", "product manager"]
    >>> }
    >>> # {'clean_name': ['list of unclean names']}
    >>> keyword_processor.add_keywords_from_dict(keyword_dict)
    >>> # Or add keywords from a list:
    >>> keyword_processor.add_keywords_from_list(["java", "python"])
    >>> keyword_processor.extract_keywords('I am a product manager for a java_2e platform')
    >>> # output ['product management', 'java']

To Remove keywords
    >>> from flashtext import KeywordProcessor
    >>> keyword_processor = KeywordProcessor()
    >>> keyword_dict = {
    >>>     "java": ["java_2e", "java programing"],
    >>>     "product management": ["PM", "product manager"]
    >>> }
    >>> keyword_processor.add_keywords_from_dict(keyword_dict)
    >>> print(keyword_processor.extract_keywords('I am a product manager for a java_2e platform'))
    >>> # output ['product management', 'java']
    >>> keyword_processor.remove_keyword('java_2e')
    >>> # you can also remove keywords from a list/ dictionary
    >>> keyword_processor.remove_keywords_from_dict({"product management": ["PM"]})
    >>> keyword_processor.remove_keywords_from_list(["java programing"])
    >>> keyword_processor.extract_keywords('I am a product manager for a java_2e platform')
    >>> # output ['product management']

For detecting Word Boundary currently any character other than this `\\w` `[A-Za-z0-9_]` is considered a word boundary.

To set or add characters as part of word characters
    >>> from flashtext import KeywordProcessor
    >>> keyword_processor = KeywordProcessor()
    >>> keyword_processor.add_keyword('Big Apple')
    >>> print(keyword_processor.extract_keywords('I love Big Apple/Bay Area.'))
    >>> # ['Big Apple']
    >>> keyword_processor.add_non_word_boundary('/')
    >>> print(keyword_processor.extract_keywords('I love Big Apple/Bay Area.'))
    >>> # []


API doc
-------

.. toctree::
    :maxdepth: 2
    :caption: KeywordProcessor

    api
    keyword_processor


Test
----
::

    $ git clone https://github.com/vi3k6i5/flashtext
    $ cd flashtext
    $ pip install pytest
    $ python setup.py test


Build Docs
----------
::

    $ git clone https://github.com/vi3k6i5/flashtext
    $ cd flashtext/docs
    $ pip install sphinx
    $ make html
    $ # open _build/html/index.html in browser to view it locally


Why not Regex?
--------------

It's a custom algorithm based on `Aho-Corasick algorithm
<https://en.wikipedia.org/wiki/Aho%E2%80%93Corasick_algorithm>`_ and `Trie Dictionary
<https://en.wikipedia.org/wiki/Trie Dictionary>`_.

.. image:: https://github.com/vi3k6i5/flashtext/raw/master/benchmark.png
   :target: https://twitter.com/RadimRehurek/status/904989624589803520
   :alt: Benchmark


Time taken by FlashText to find terms in comparison to Regex.

.. image:: https://thepracticaldev.s3.amazonaws.com/i/xruf50n6z1r37ti8rd89.png


Time taken by FlashText to replace terms in comparison to Regex.

.. image:: https://thepracticaldev.s3.amazonaws.com/i/k44ghwp8o712dm58debj.png

Link to code for benchmarking the `Find Feature <https://gist.github.com/vi3k6i5/604eefd92866d081cfa19f862224e4a0>`_ and `Replace Feature <https://gist.github.com/vi3k6i5/dc3335ee46ab9f650b19885e8ade6c7a>`_.

The idea for this library came from the following `StackOverflow question
<https://stackoverflow.com/questions/44178449/regex-replace-is-taking-time-for-millions-of-documents-how-to-make-it-faster>`_.


References
----------

The original paper published on `FlashText algorithm <https://arxiv.org/abs/1711.00046>`_.

The article published on `Medium freeCodeCamp <https://medium.freecodecamp.org/regex-was-taking-5-days-flashtext-does-it-in-15-minutes-55f04411025f>`_.


Contribute
----------

- Issue Tracker: https://github.com/vi3k6i5/flashtext/issues
- Source Code: https://github.com/vi3k6i5/flashtext/


License
-------

The project is licensed under the MIT license.
