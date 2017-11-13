# Portmanteau Bot

## What is [u/PORTMANTEAU-BOT](https://www.reddit.com/u/PORTMANTEAU-BOT)?

It's a Reddit bot which automatically generates portmanteaux of 2-word Reddit comments and posts them as replies.

## What's a portmanteau?

From [Wikipedia](https://en.wikipedia.org/wiki/Portmanteau):

> A portmanteau (/pɔːrtˈmæntoʊ/, /ˌpɔːrtmænˈtoʊ/; plural portmanteaus or portmanteaux /-ˈtoʊz/) or portmanteau word is a linguistic blend of words, in which parts of multiple words or their phones (sounds) are combined into a new word, as in smog, coined by blending smoke and fog, or motel, from motor and hotel. In linguistics, a portmanteau is defined as a single morph that represents two or more morphemes.

## What's in this repo?

The only thing included here is the core function to generate portmanteaux - the quality control functions (and the dictionaries they use) are kept private to protect the bot from having unsavoury words put in its mouth.

## Why didn't the bot reply to my 2-word comment?

If it was a 2-word comment with no weird punctuation, it must have failed a quality check. Perhaps one of the words in your comment was less than 5 characters. Perhaps your comment had a mundane word in it like "thanks", "please", or it was someone's name. Perhaps it contained bad language or produced a portmanteau that did. The bot constantly has hundreds of comments to choose from and so vetoes the vast majority of them (see the list of quality controls below).

## How does the bot work?

The bot streams [all comments from across Reddit](https://www.reddit.com/r/all/comments/) and looks for 2-word comments. It does some quality control (see below) which vetoes most of those comments as bad portmanteau fodder. If it finds a good comment, it constructs a portmanteau and applies some more quality control checks, abandoning any bad results. Finally, it posts the portmanteaux that make it through. It also checks its post history, deleting its downvoted comments and [defending its honour](https://www.reddit.com/r/fakealbumcovers/comments/72hgo4/wizard_people_lets_be_poor/dnixkbw/?context=3) from unfair human harassment.

## What kind of quality controls are there?

There are two sets of filters: one on the comments read from Reddit as inputs, and one on the output portmanteaux that are considered for posting:

**Controls on input**

1. Require each of the 2 words to be at least 5 letters long

2. Veto comments that are just people's names, using a list of common first names.

3. Veto comments that contain "bad" words or fragments of words, using a list of commmon chatter such as "thank", "hahaha", "where", etc, as well as curses and slurs.

4. Veto comments where 1 of the words has a "bad" ending such as "-ing", as these tend to make meaningless portmanteaux.

**Controls on output**

1. Veto portmanteaux which are shorter than the shortest input word.

2. Veto portmanteaux which contain slurs.

## How are the portmanteaux constructed?

Here's a brief summary of the procedure, for which you can read the source code in the function [makepm()](demoPM.py#L71):

1. Look for 3-letter strings in common - if found, stitch the words together over those. Otherwise, continue.

2. Find & record the positions of all vowels in string A and string B.

3. Look for common vowels between the two strings (excluding any matches that fall in the first 2 letters of string A or the last 2 letters of string B, in order to preserve as much of the original strings as possible).

4. If common vowels pass these tests, their positions are used to split the original strings, which are then concatenated about the shared vowel.

5. If no common vowels are found to pass these tests, any vowel pair is considered.

6. Preference is given to late vowels in string A and early vowels in string B, in order to preserve as much of the original strings as possible.

7. Once vowels are selected, their positions are used to split the original strings, which are then concatenated about the vowel from string B.

8. Priority is given to the vowel from string B and it replaces the vowel from string A, as this helps the portmanteau rhyme better with the original phrase.

## Can I see the code that does this?

Sure - you can read it or try it out yourself by cloning this repo and using the script [demoPM.py](demoPM.py), for example:

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

## Where does the bot live?

It used to be confined to an old Ubuntu laptop under my bed, but thanks to the [GitHub student developer pack](https://education.github.com/pack) and [AWS educate](https://aws.amazon.com/education/awseducate/), it has ascended (for free!) to the divine halls of Amazon Web Services where it shall live forever more... or at least until the next time [AWS breaks](https://www.recode.net/2017/3/2/14792636/amazon-aws-internet-outage-cause-human-error-incorrect-command).

## How well does it work?

You tell me - the bot's best efforts are viewable [here](https://www.reddit.com/user/portmanteau-bot/comments/?sort=hot).


## Any future plans?

Use the bot's post history (with associated upvotes) to train a neural network as a binary classifer to separate good and bad portmanteaux. Then, slot this in right before posting as a final quality check, hopefully improving the quality of future output.

