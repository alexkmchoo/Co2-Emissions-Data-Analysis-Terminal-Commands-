# Data Literacy Field Notes & Terminal Log

**The scenario:** A climate-and-energy nonprofit wants to use this
country-level energy and emissions dataset to help decide where to direct
community-protection and climate-support efforts — but the file just
landed with zero documentation. Before anyone can trust a decision built
on top of it, you need to figure out what's actually in it — what each
column really holds, and anything that looks off. That's what this whole
file walks through: the real, everyday first pass any analyst runs on an
unfamiliar file, and the real answers you need before this data is safe
to act on.

Dataset: `../data/co2_emissions_2022.csv` (see `../data/SOURCE.md`). Each
section below describes what you're trying to find out — **figuring out
which command (and which flags) actually gets you there is part of what
this project tests**, so the exact command isn't given. Where a command
belongs, you'll see a `# TODO:` comment describing the goal instead of a
ready-to-run command; replace the comment with the real command you used.
Paste the **real output** you got underneath, then write your answer based
on what you actually saw, not what you'd expect. Work through this in
order; open the raw file before anything else.

Run all commands from the repo root — the top-level folder you land in
after cloning *your own* copy of this repo (`git clone ...` then `cd
your-repo-name`, see `README.md` Step 3; not this template) — or `cd` into
`data/` first and drop the `data/` prefix — your choice, just be
consistent.

**This one file is your whole record** — the commands you ran, their real
output, and your written answers, all together in the order you actually
did the work. There's no separate log file to keep in sync with this one;
whenever you write a command down anywhere in this file, use a fenced code
block tagged `bash`, like this:

````
```bash
git status
```
````

Three backticks, the word `bash`, your command, three backticks to close —
it renders with syntax highlighting and makes the command copy-pasteable.

## Setup log

Before jumping into the dataset, log the commands you actually ran to set
this repo up (see `README.md`) — `git init`/clone, creating your
`.gitignore`, your first commit. Add to this as you go, don't
reconstruct it from memory at the end.

```bash
git clone <repo url>
cd <repo>
```

## 1. First look at the raw file

Before trusting anything about this file's structure, you want a quick,
safe peek at it — not the whole 254-row file, just the **first 5 lines**,
enough to see the shape of it without scrolling past everything.

```bash
cd data/
head -n 5 co2_emissions_2022.csv
```

**Output:**

country,iso_code,year,population,gdp,co2
Afghanistan,AFG,2022,40578847,53303471617,10.170
Africa,,2022,1445862695,,1459.114
Africa (GCP),,2022,,,1459.103

**What I notice:** 

Africa has missing values in the iso_code column and the gdp column. Africa (GCP) also has missing values in the iso_code, population, and gdp column columns.

## 2. Column names

Next you want to see just the column names by themselves — one per line is
a lot easier to scan than one long comma-separated row.

```bash
cd data/
head -n 1 co2_emissions_2022.csv
```

**Output:**

country,iso_code,year,population,gdp,co2

## 3. Column-by-column data type table

For each column, you want to preview real values — **the first 6 values**
of just that one column — without scrolling through every field on every
row. Column numbers for reference: `country`=1, `iso_code`=2, `year`=3,
`population`=4, `gdp`=5, `co2`=6.

```bash
cd data/
cut -d',' -f1 co2_emissions_2022.csv | head -n 6
cut -d',' -f2 co2_emissions_2022.csv | head -n 6
cut -d',' -f3 co2_emissions_2022.csv | head -n 6
cut -d',' -f4 co2_emissions_2022.csv | head -n 6
cut -d',' -f5 co2_emissions_2022.csv | head -n 6
cut -d',' -f6 co2_emissions_2022.csv | head -n 6
```

Fill in the table from what you actually saw — not a guess from the column
name alone:

| Column | Inferred type | How I can tell |
| ------ | -------- | --------- |
| country | string | Pure text. Text labeling the country |
| iso_code | string | Pure text. Its a text based label |
| year | string | Even though this are numbers, no arthmetic will be done on it. It is used as a label. Number based label |
| population | integer | Population represents a quantity. Arthmetic can be performed here|
| gdp | integer | gdp also represents a quantity. Arthmetic can also be performed here |
| co2 | float | contains decimal points in the numbers |

**In your own words:** what's the actual difference between a list/array
and a table? (Not a definition you looked up — explain it the way you'd
explain it to a friend who's never coded. No command needed for this one,
just think about the table you just filled in above.)

A table consists of rows and columns with data inside whereas a list/array contains data seperated by just commas.

## 4. Structured or unstructured?

State whether this dataset is structured or unstructured, and why.

This dataset is structured because not only does it has rows and columns, the same data type is in each column. 

## 5. Before you trust this data

Your first look (see section 1) turned up a row with a suspiciously
**blank** `iso_code`. Before you build anything on this data, you want to
know exactly how many of the 254 rows share that blank — is it a one-off,
or a real pattern worth flagging before anyone draws conclusions from it?
You may need more than one command for this — piping (`|`) feeds one
command's output into the next as input.

```bash
cut -d',' -f2 co2_emissions_2022.csv | grep -E '^ *$' | wc -l
```

**Output:**

36  
There are 36 blanks or missing data in the iso_code column

```bash
cut -d',' -f5 co2_emissions_2022.csv | grep -E '^ *$' | wc -l
```

**Output:**

89 
There are 89 blanks or missing data in the gdp column

**Additional !** 
```bash
cut -d',' -f4 co2_emissions_2022.csv | grep -E '^ *$' | wc -l
```

**Output:**

25
There are 25 blanks or missing data in the population column

List **at least 2** specific things you'd want to know about how this
dataset was collected, before trusting any conclusion drawn from it — ground
at least one in the counts you just found.

**My 2 concerns:**
1. Why were some countries seperated from their continent? Asia has a value but so does Malaysia and Japan
2. How were the missing data values handeled? like the 89 missing values from the gdp column

## 6. Data-quality question for the source

Take the **most specific and checkable** of the two concerns above, and
phrase it as a real question you'd send to the data's source/maintainer. A
good test: could they actually answer it with a concrete yes/no or fact —
not "is this data good?"

About 35% of the values are missing in the GDP column (89 missing entries out of 254) of this dataset, which could affect my analysis. Was this missing data intentionally omitted or was it just a collection error?

## 7. Something you're actually curious about

Different question from #6 — that one was about checking the data's
trustworthiness. This one's about you: what's something you're genuinely
curious to explore using this data? Pick two specific countries you're
curious to compare, and look up their full rows.

```bash
head -n 1 co2_emissions_2022.csv && grep -E "Ukraine|Russia" co2_emissions_2022.csv
```

**Output:**

country,iso_code,year,population,gdp,co2  
Russia,RUS,2022,145579889,3731440829579,1675.461  
Ukraine,UKR,2022,41048768,309166674673,143.040

**My question:** 

I wanted to look at gdp values and co2 values across these two countries because they've been at war since the year the data was collected.
My question is How does the population, gdp, and co2 compare across Russia and Ukraine? Who has the higher values and what does that tell you about their standings.

**Another thing I wanted to check is:**

```bash
cut -d',' -f1,5,6 co2_emissions_2022.csv 
```

**Output:**
country,gdp,co2 
Afghanistan,53303471617,10.170  
Africa,,1459.114  
Africa (GCP),,1459.103  
Albania,36171005594,4.498 
Algeria,595820067103,192.779  
Andorra,,0.423  
...
Venezuela,149682062815,108.424
Vietnam,833803909046,322.653
Wallis and Futuna,,0.029
World,130112562171125,37527.773
Yemen,60852995288,10.404
Zambia,68469600029,10.939
Zimbabwe,25901585881,12.232

Now I didnt include the whole output and skipped a little because there are 254 lines

**My question**

The question I wanted to explore with this command is just to look at how the gdp column compares with the co2 emission levels. Do countries with higher gdp have higher co2 emissons? What is the correlation here, if any?


## 8. AI-literacy reflection

Name **2 concrete risks** of trusting an AI assistant's output about this
dataset (or any data question) without checking it yourself. Then state
**one specific personal rule** for when you'll double-check an AI-provided
fact before relying on it professionally — specific enough that you could
actually follow it, not "I'll be careful."

**Risks:**
1. It could give you the wrong information like there are 900 blanks in this row. This is also known as a hallucination
2. It could read the data question wrong, thus even if the answer seems right, its completely off topic

**My rule:** 

Rather than uploading your dataset to AI and it to give you the answer, ask AI like a tutor to teach you how to do it
Example : We learned grep, &&, and | in class already. You could ask AI 
"I know head -n1 will pull the first row of data on the output, and grep -E will search for a specific value, how could I combine these to specifically look at two different rows only in my dataset".

## 9. Something that went wrong (and how you figured it out)

Somewhere in this project you're going to run a command that either throws
an error or gives you output that isn't what you expected — everyone hits
this at least once. Paste the actual command and what actually happened
(not a paraphrase), then explain what that told you and how it led to the
fix — not "I googled it and it worked."

**NUMBER ONE**
```bash
cut -d',' -f2,4,5 co2_emissions_2022.csv | grep -E '^ *$' | wc -l
```

**What happened:**

It returned "0", which I know is false because from looking at the data, blanks definitely exist, so I know something went wrong in my command

**What it told me and how I fixed it:** 

This was my original command
```bash
cut -d',' -f2 co2_emissions_2022.csv | grep -E '^ *$' | wc -l
```
I decided to combine this because we learned in class this command works :
```bash
cut -d’,’ -f1,3,5 <file.name>
```
So I thought, why not do the same but use grep -E to look for blanks across rows 2, 4, and 5.

How I fixed it was actually playing around with it and copy and pasting the same code again, just with a different f number value
```bash
cut -d',' -f2 co2_emissions_2022.csv | grep -E '^ *$' | wc -l && cut -d',' -f5 co2_emissions_2022.csv | grep -E '^ *$' | wc -l
```

Although I did not include this command in question 5 (since q5 was asking for two seperate commands), this shows a single command to find the rows of blanks across columns 2 and 5.
 
**NUMBER TWO**
```bash
grep "Russia" co2_emissions_2022.csv && grep "Ukraine" co2_emissions_2022.csv
```
**What happened**

It returned cmdanddquote>. I tried to play around with it, replacing "&&" with "|" , which gave me pipe dquote>.

**What it told me and how I fixed it:** 

It told me that my commands were wrong. We learned in class that | means or and && means and. I used claude to tell me what went wrong. I put "How can I combine these two into one single line. Here is what I have so far <command>, I also tried <command2 with |>. It told me what I needed to do and I was actually on the right track.

So I fixed it by inserting -E in front of grep and closing the quotation with | between the two strings I wanted to look up.

Revised Code:
```bash
grep -E "Ukraine|Russia" co2_emissions_2022.csv
```
This yielded an output of Russia and Ukranie's information

But I wasn't finished, I also wanted to include the header, which describes what I am looking at. Again we learned in class that && combines commands so,I added && head n -1 <file.name> to the end, but realized I needed to add it to the front.

Final code: 
```bash
head -n 1 co2_emissions_2022.csv && grep -E "Ukraine|Russia" co2_emissions_2022.csv
```

 I just thought this was cool and experimenting what we learned in class actually worked. Shoutout Tina for always telling us to try it!

## Above and Beyond
**Chain Terminal Commands:**
```bash
cut -d',' -f2 co2_emissions_2022.csv | grep -E '^ *$' | wc -l && cut -d',' -f5 co2_emissions_2022.csv | grep -E '^ *$' | wc -l
```

Same command as question 9. What it does is combines the intial cut -d command, which filters it to column 2, grep -E, which finds a specific value for me, and wc -l which counts the rows. This was used with a "&&" function which can combine two commands, which I found the blanks for columns 2 (iso_code) and 4 (gdp) together.
let's say I also want to find the blanks in population as well. I would just add this to the command:
```bash
cut -d',' -f2 co2_emissions_2022.csv | grep -E '^ *$' | wc -l && cut -d',' -f4 co2_emissions_2022.csv | grep -E '^ *$' | wc -l && cut -d',' -f5 co2_emissions_2022.csv | grep -E '^ *$' | wc -l
```
The output gives me :
36 
25
89

**Preview of Module 1:**

After running this python code: 
```
def guess_type(value):
    """Given one value (as a string), guess what type of data it is."""
    if value == "":
        return "missing"
    try:
        int(value)
        return "whole number"
    except ValueError:
        pass
    try:
        float(value)
        return "decimal number"
    except ValueError:
        pass
    return "text"

print(guess_type("40578847"))    # a population value
print(guess_type("10.170"))      # a co2 value
print(guess_type("Afghanistan")) # a country name
print(guess_type(""))       
```
This was the outcome : 

whole number  
decimal  
text  
missing  

guess_type() does agree with my own column-type table from section 3 because it states the same data type as I did because you can see that if value is int(value) then it will return a "whole number", if it is a float(value) then it will return a "decimal number" and Afghanistan returned "text" which is a string. The only thing that wasn't mentioned was the missing value column in my section 3.
