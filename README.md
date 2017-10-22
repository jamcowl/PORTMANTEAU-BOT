# PortmanteauBot

### FAQ

**WTF is this?**

This is a public repo showing some of the code for [/u/PORTMANTEAU-BOT](www.reddit.com/u/PORTMANTEAU-BOT), a Reddit bot which automatically generates portmanteaux of 2-word Reddit comments and posts them as replies. The only thing included in this repo is the core function to generate portmanteaux - the quality control checks (including their dictionaries) are kept private.

From [Wikipedia](https://en.wikipedia.org/wiki/Portmanteau):

> A portmanteau (/pɔːrtˈmæntoʊ/, /ˌpɔːrtmænˈtoʊ/; plural portmanteaus or portmanteaux /-ˈtoʊz/) or portmanteau word is a linguistic blend of words, in which parts of multiple words or their phones (sounds) are combined into a new word, as in smog, coined by blending smoke and fog, or motel, from motor and hotel. In linguistics, a portmanteau is defined as a single morph that represents two or more morphemes.

### Functionality

The bot streams [all comments from across Reddit](https://www.reddit.com/r/all/comments/) and looks for 2-word comments. It does some quality control (see below) which vetoes most of those comments as bad portmanteau fodder. Then it constructs a portmanteau of a good comment and applies some more quality control to the output, vetoing the bad results. Finally, it posts any portmanteaux that make it through.

It also checks its post history, deleting its downvoted comments and [defending its honour](https://www.reddit.com/r/movies/comments/7383ja/new_actors_who_will_become_big_in_the_next_510/dnokcd4/?context=3) from unfair human harassment.


### Quality controls

##### On input

1. Require each of the 2 words to be at least 5 letters long

2. Veto comments that are just people's names, using a list of common first names.

3. Veto comments that contain "bad" words or fragments of words, using a list of commmon chatter such as "thank", "hahaha", "where", etc, as well as curses and slurs.

4. Veto comments where 1 of the words has a "bad" ending such as "-ing", as these tend to make meaningless portmanteaux.

##### On output

1. Veto portmanteaux which are shorter than the shortest input word.

2. Veto portmanteaux which contain slurs.


### Portmanteau creation function

Here's a brief summary of its flow:

1. Look for 3-letter strings in common - if found, stitch the words together over those. Otherwise, continue.

2. Find & record the positions of all vowels in string A and string B.

3. Look for common vowels between the two strings (excluding any matches that fall in the first 2 letters of string A or the last 2 letters of string B, in order to preserve as much of the original strings as possible).

4. If common vowels pass these tests, their positions are used to split the original strings, which are then concatenated about the shared vowel.

5. If no common vowels are found to pass these tests, any vowel pair is considered.

6. Preference is given to late vowels in string A and early vowels in string B, in order to preserve as much of the original strings as possible.

7. Once vowels are selected, their positions are used to split the original strings, which are then concatenated about the vowel from string B.

8. Priority is given to the vowel from string B and it replaces the vowel from string A, as this helps the portmanteau rhyme better with the original phrase.


### Example Usage

The portmanteau function can be used in isolation from the command line, using the script [demoPM.py](demoPM.py), for example:

```bash
$> ./demoPM.py labrador poodle
Output portmanteau is 'labradoodle'
```

```bash
$> ./demoPM.py mock documentary
Output portmanteau is 'mocumentary'
```

```bash
$> ./demoPM.py television evangelist
Output portmanteau is 'televangelist'
```

```bash
$> ./demoPM.py flabby hamster
Output portmanteau is 'flamster'
```


### Results

Comments are viewable [here](https://www.reddit.com/user/PORTMANTEAU-BOT/comments).


### TODO

Train a neural network to classify good and bad portmanteaux based on the bots post history and upvotes. Use this classifier as a final veto before posting to improve future output.
