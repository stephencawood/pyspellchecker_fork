# pyspellchecker

[![Build Status](https://travis-ci.org/barrust/pyspellchecker.svg?branch=master)](https://travis-ci.org/barrust/pyspellchecker)
[![Coverage Status](https://coveralls.io/repos/github/barrust/pyspellchecker/badge.svg?branch=master)](https://coveralls.io/github/barrust/pyspellchecker?branch=master)

Pure Python Spell Checking based on
[Peter Norvig's](https://norvig.com/spell-correct.html) blog post on setting up
a simple spell checking algorithm.

It uses a [Levenshtein Distance](https://en.wikipedia.org/wiki/Levenshtein_distance)
algorithm to find permutations within an edit distance of 2 from the
original word. It then compares all permutations (insertions, deletions,
replacements, and transpositions) to known words in a word frequency list.
Those words that are found more often in the frequency list are `more likely`
the correct results.


## Installation

The easiest method to install is using pip:

``` bash
pip install pyspellchecker
```

To install from source:
``` bash
git clone https://github.com/barrust/pyspellchecker.git
cd pyspellchecker
python setup.py install
```

As always, I highly recommend using the [Pipenv](https://github.com/pypa/pipenv)
package to help manage dependencies!

## Quickstart

After installation, using pyspellchecker should be fairly straight forward:

``` python
from spellchecker import SpellChecker


spell = SpellChecker()

# find those words that may be misspelled
misspelled = spell.unknown(['something', 'is', 'hapenning', 'here'])

for word in misspelled:
    # Get the one `most likely` answer
    print(spell.correction(word))

    # Get a list of `likely` options
    print(spell.candidates(word))
```

If the Word Frequency list is not to your liking, you can add additional text
to generate a more appropriate list for your use case.

``` python
from spellChecker import SpellChecker

spell = SpellChecker()  # loads default word frequency list
spell.word_frequency.load_text_file('./my_free_text_doc.txt')

# if I just want to make sure some words are not flagged as misspelled
spell.word_frequency.load_words(['microsoft', 'apple', 'google'])
spell.known(['microsoft', 'google'])  # will return both now!
```

More work in storing and loading word frequency lists is planned; stay tuned. 

## Additional Methods
On-line documentation is in the future; until then you can find SpellChecker
here:

`correction(word)`: Returns the most probable result for the misspelled word

`candidates(word)`: Returns a set of possible candidates for the misspelled
word

`known([words])`: Returns those words that are in the word frequency list

`unknown([words])`: Returns those words that are not in the frequency list

`word_probability(word)`: The frequency of the given word out of all words in
the frequency list

#### The following are less likely to be needed by the user but are available:

`edit_distance_1(word)`: Returns a set of all strings at a Levenshtein Distance
of one

`edit_distance_2(word)`: Returns a set of all strings at a Levenshtein Distance
of two
