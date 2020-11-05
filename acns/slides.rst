.. include:: <s5defs.txt>

(hit the right-arrow key to go to the next slide)

BLAKE2 — an alternative to MD5/SHA-1
====================================

Jean-Philippe Aumasson, Samuel Neves, Zooko Wilcox-O'Hearn (that's me!), and Christian Winnerlein

https://blake2.net

 .. Thank you
 .. 20 minutes
 .. advertisement for reading the paper
 .. BLAKE2 is important, so I should study it and try to break it or prove its security

An unmet need
=============

.. We set out to create a modern competitor for the most widely-used
   hash functions in the world. SHA-2/SHA-3? No. MD5/SHA-1!

What are SHA-2/SHA-3 used for?
------------------------------

.. class:: incremental

 public key digital signatures, HMAC

 small inputs, time-insensitive, by cryptographers

..  — a few bytes up to megabytes, e.g. public key certificates, TCP/IP packets, documents, …
.. * hash function is typically not in the critical path for performance
.. the performance bottleneck is often the public-key digital signature algorithm or the network

What are MD5/SHA-1 used for?
----------------------------

.. class:: incremental

 Distributed filesystems, cloud storage, data deduplication,
 peer-to-peer file-sharing, revision control systems, host-based
 intrusion detection systems, digital forensics, distributed
 databases, …

.. * input is often large — can be up to petabytes!
..  * hash function is sometimes in the critical path for performance
.. * designer is usually not educated in cryptography

 examples: OpenStack, git, ZFS, Perforce, md5sum, BitTorrent,
 Tahoe-LAFS *(that's me again)*, WinRAR, couchdb, Samhain, NIST
 recommendations for digital forensics (NIST SP 800-86)

An unmet need
=============

What are SHA-2/SHA-3 used for?
------------------------------

What are MD5/SHA-1 used for?
----------------------------

large inputs, time-sensitive, by engineers
 
.. class:: incremental

 examples: OpenStack, git, ZFS, Perforce, md5sum, BitTorrent,
 Tahoe-LAFS (that's me again), WinRAR, couchdb, Samhain, NIST
 recommendations for digital forensics (NIST SP 800-86)

.. Perforce customer (Pixar?) runs the data-integrity verification
   task, it takes a week to run. What hash algorithm does Perforce
   use? MD5

Why use BLAKE1?
===============

.. We started with BLAKE1 because we have high confidence in its security.

* Salsa20 (2005), eSTREAM (co-winner)
* BLAKE1 (2009), SHA-3 contest (finalist)

.. class:: incremental

 from *Third-Round Report of the SHA-3 Cryptographic Hash Algorithm Competition*:

 “BLAKE and Keccak have very large security margins.”

 “Keccak received a significant amount of cryptanalysis, although not
 quite the depth of analysis applied to BLAKE.”

Why use BLAKE1?
===============

.. class:: incremental

 from *Third-Round Report of the SHA-3 Cryptographic Hash Algorithm Competition*:

 “Skein and BLAKE have the best overall software performance.”

Why didn't NIST use BLAKE1 for SHA-3?
=====================================

.. So why did NIST choose Keccak instead of BLAKE1 to be the final
   SHA-3 standard? Because the purpose of the SHA-3 contest was to
   create a replacement for SHA-2 in case SHA-2 gets broken! BLAKE1
   was too similar to SHA-2.

.. class:: incremental

 from *Third-Round Report of the SHA-3 Cryptographic Hash Algorithm Competition* by NIST:

 “desire for SHA-3 to complement the existing SHA-2 algorithms … BLAKE
 is rather similar to SHA-2.”

From BLAKE1 to BLAKE2
=====================

* reducing the number of rounds
* tweaks (rotation constants, minimal padding, fewer constants, little-endian)
* parallel mode
* tree mode

.. class:: incremental

 BLAKE1 had 14 rounds [*]; BLAKE2 has 10

results
=======

 cycles per byte on Intel Core i5-3210M (Ivy Bridge)

 +----------+----------+--------+------+
 | function | cycles per byte          |
 +          +----------+--------+------+
 |          | long msg | 4096 B | 64 B |
 +==========+==========+========+======+
 | MD5      |      5.0 |    5.2 | 13.1 |
 +----------+----------+--------+------+
 | SHA1     |      4.7 |    4.8 | 13.7 |
 +----------+----------+--------+------+
 | SHA256   |     12.8 |   13.0 | 30.0 |
 +----------+----------+--------+------+
 | Keccak   |      8.2 |    8.5 | 26.0 |
 +----------+----------+--------+------+
 | BLAKE1   |      5.8 |    6.0 | 14.9 |
 +----------+----------+--------+------+
 | BLAKE2   |      3.5 |    3.5 |  9.3 |
 +----------+----------+--------+------+

parallel mode
=============

.. figure:: blake2-parallel.png
   :width: 100%
   :figwidth: image

The first users
===============

* compression/archiver tools: WinRAR, Pcompress
* alternative implementations and language bindings: Go, Python, Dart, Node.js, Javascript, PHP…

Go to https://blake2.net to news and information.

Publish papers analyzing BLAKE2's security!

.. notes for questions: paper measuring impact of hash function efficiency on Tahoe-LAFS
