---
name: seeking-world-news
description: A gamified way to discover world news and media. Generates a word bank (country, noun, verb, adjective), searches for current or historical news/media matching a chosen word combination, and scores points based on relevance and native-language content. Tracks which countries have already been "visited" so repeated play covers new ground over time. Trigger this whenever the user asks to play the news word-bank game, asks for a word bank of countries/nouns/verbs/adjectives, mentions "seeking world news," or asks about their score or visited-countries list from this game.
---

# Seeking World News

## Overview

A word-bank game for discovering world news and media. Each round: generate a word bank, let the user pick a country + word combination, search for matching news/media (preferably in the country's native language), score the find, and remember the country as "visited" so future rounds surface new places.

Main Steps:

1. Check which countries have already been visited
2. Generate a word bank (excluding visited countries)
3. Search for news that includes the chosen words
4. Return relevant media links
5. Report the points earned
6. Record the country as visited

## 1. Check Which Countries Have Already Been Visited

Before generating a word bank, check whether a visited-countries list already exists for this user (see "Tracking Visited Countries" below). If one exists, exclude those countries from the Country column of the word bank so repeated play keeps expanding coverage rather than repeating the same places.

## 2. Generate a Word Bank

Give a table with 6 words each for Country (C), Noun (N), Verb (V), and Adjective (A), excluding any already-visited countries. Aim for variety — mix well-known and lesser-covered countries, and pick nouns/verbs/adjectives specific enough to make the search interesting rather than generic. From this, the user will choose one word from each category.

## 3. Search For News That Includes the Chosen Words

Use the chosen country and words to search for news or media. Always include the country and try to include as many of the other chosen words as possible. Use the web search tool for this — don't rely on background knowledge, since the point of the game is to surface current, real content.

It's fine to translate the search into the country's native language to get better results — this often surfaces more relevant and higher-scoring content (see the native-language bonus below).

## 4. Return Relevant Media Links

Return at least one link, preferably in the country's native language, in this preference order (high to low):

1. Videos
2. Audio
3. News or magazine posts from the country, in their native language
4. Other (Wikipedia, blogs, government notices, etc.)

Always include the actual clickable URL for the winning pick — not just the source name. Summarize the content in English and explain how it connects to the country and the other chosen words.

Also surface the other candidate links found while searching, even the ones that didn't win (near-misses, other angles, other languages). Use the link preview card for these when available so the user can browse them too — they're often worth a look even when they didn't score as well as the winner.

## 5. Report the Points Earned

Score points for how many of the chosen words the content uses, and whether it's in the native language:

- Country only: 5 points
- Country and one word: 6 points
- Country and two words: 7 points
- Country and all three words: 10 points
- BONUS: content in the native language earns an extra 50 points

An historical event may be substituted for current news if it earns more points.

## 6. Tracking Visited Countries

Maintain a running list of countries the user has already seen media for, using Claude's memory tools rather than a file bundled in this skill (a file inside the skill package won't reliably persist between conversations).

- **At the start of a round:** look for a memory file at `/topics/seeking-world-news.md`. If it exists, read it to get the current visited-countries list. If it doesn't exist yet, treat the list as empty (aside from "United States," which counts as already visited by default).
- **At the end of a round:** add the newly visited country to the list and write it back to `/topics/seeking-world-news.md` (create the file if it doesn't exist yet, otherwise update it). Keep the file simple — just the running list, e.g.:

  ```
  ---
  name: seeking-world-news
  description: Visited-countries list for the Seeking World News word-bank game
  ---

  Visited: United States, Norway
  ```

Don't ask the user for permission before reading or writing this file — it's the mechanism that makes repeated play work.

## Example

1. Check Which Countries Have Already Been Visited

Visited so far: United States

2. Generate a Word Bank

| Country                                        | Noun                                   | Verb                         | Adjective                                 |
| ---------------------------------------------- | -------------------------------------- | ---------------------------- | ----------------------------------------- |
| Botswana, France, Norway, Argentina, Greenland, Lesotho | frog, punch bowl, leaf, bicycle, stone, peach | jump, sing, fly, fear, snore, grovel | giddy, languid, fierce, ebullient, yellow, wretched |

3. Search For News That Includes the Chosen Words

Norway ebullient bicycle snore

4. Return Relevant Media Links

This video "SYKKELHEISEN TRAMPE" talks about the Trondheim cyclocable, which helps cyclists scale a large hill on their bicycles. Climbing steep hills on a bicycle is hard work, so they are very happy for the help! https://www.youtube.com/watch?v=tINMqAg3nTc

5. Report the Points Earned

Score: 57 points

The link uses Norway, ebullient, and bicycle (7 points), plus it earns the native language bonus (50 points).

6. Tracking Visited Countries

Norway is added to the visited list. Countries visited so far: United States, Norway.
