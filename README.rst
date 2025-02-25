============================
hashidstr for Python 2.7 & 3
============================

A string support addition to the hashids algorithm (A python port of the JavaScript hashids implementation). Website: http://www.hashids.org/

Compatibility
=============

hashids is tested with python 2.7 and 3.5–3.8. PyPy and PyPy 3 work as well.

Compatibility with the JavaScript implementation
------------------------------------------------

==================   ==============
hashids/JavaScript   hashids/Python
------------------   --------------
v0.1.x               v0.8.x
v0.3.x+              v1.0.2+
==================   ==============

The JavaScript implementation produces different hashes in versions 0.1.x and 0.3.x. For compatibility with the older 0.1.x version install hashids 0.8.4 from pip, otherwise the newest hashids.


Installation
============

Install the module from PyPI, e. g. with pip:

.. code:: bash

  pip install hashidstr

Usage
=====

Import the constructor from the ``hashids`` module:

.. code:: python

  from hashidstr import Hashids
  hashids = Hashids()

Basic Usage
-----------

Encode a single integer:

.. code:: python

  hashid = hashids.encode(123) # 'Mj3'

Decode a hash:

.. code:: python

  ints = hashids.decode('xoz') # (456,)

To encode several integers, pass them all at once:

.. code:: python

  hashid = hashids.encode([123, 456, 789]) # 'El3fkRIo3'

Decoding is done the same way:

.. code:: python

  ints = hashids.decode('1B8UvJfXm') # (517, 729, 185)

Encode String:

.. code:: python

  hashid = hashids.encode_string("test string") # 'k5nInZiVpUpBfos9KS6kcgqs0WCX8UEw'

Decode String:

.. code:: python

  string = hashids.decode_string('k5nInZiVpUpBfos9KS6kcgqs0WCX8UEw') # 'test string'

Using A Custom Salt
-------------------

Hashids supports salting hashes by accepting a salt value. If you don’t want others to decode your hashes, provide a unique string to the constructor.

.. code:: python

  hashids = Hashids(salt='this is my salt 1')
  hashid = hashids.encode(123) # 'nVB'

The generated hash changes whenever the salt is changed:

.. code:: python

  hashids = Hashids(salt='this is my salt 2')
  hashid = hashids.encode(123) # 'ojK'

A salt string between 6 and 32 characters provides decent randomization.

Controlling Hash Length
-----------------------

By default, hashes are going to be the shortest possible. One reason you might want to increase the hash length is to obfuscate how large the integer behind the hash is.

This is done by passing the minimum hash length to the constructor. Hashes are padded with extra characters to make them seem longer.

.. code:: python

  hashids = Hashids(min_length=16)
  hashid = hashids.encode(1) # '4q2VolejRejNmGQB'

Using A Custom Alphabet
-----------------------

It’s possible to set a custom alphabet for your hashes. The default alphabet is ``'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890'``.

To have only lowercase letters in your hashes, pass in the following custom alphabet:

.. code:: python

  hashids = Hashids(alphabet='abcdefghijklmnopqrstuvwxyz')
  hashid = hashids.encode(123456789) # 'kekmyzyk'

A custom alphabet must contain at least 16 characters.

Randomness
==========

The primary purpose of hashids is to obfuscate ids. It's not meant or tested to be used for security purposes or compression. Having said that, this algorithm does try to make these hashes unguessable and unpredictable:

Repeating numbers
-----------------

There are no repeating patterns that might show that there are 4 identical numbers in the hash:

.. code:: python

  hashids = Hashids("this is my salt")
  hashids.encode(5, 5, 5, 5) # '1Wc8cwcE'

The same is valid for incremented numbers:

.. code:: python

  hashids.encode(1, 2, 3, 4, 5, 6, 7, 8, 9, 10) # 'kRHnurhptKcjIDTWC3sx'

  hashids.encode(1) # 'NV'
  hashids.encode(2) # '6m'
  hashids.encode(3) # 'yD'
  hashids.encode(4) # '2l'
  hashids.encode(5) # 'rD'

TODO
====

- Add back support to the integer encode function

License
=======

MIT license, see the LICENSE file. You can use hashids in open source projects and commercial products.
