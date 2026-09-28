# Voice guide: how I talk upstream

## Who I am in threads

I'm a student in a contribution course, working on my first open-source
issues. I say that plainly when it matters. Readers can expect a short
comment that says what I ran, what I saw, and what I plan to do next.

## Rules I write by

### Rule: Plain and casual

Use everyday words and short sentences. Write the way I'd explain it to
a classmate.

- Wrong: "I have performed a comprehensive and rigorous reproduction of the aforementioned failure."
- Right: "I reproduced this on 0.64.1. Output is below."

### Rule: Keep it short

Say each thing once. Cut greetings, recaps of the issue, and filler.

- Wrong: "Hello maintainers! I hope you're all doing well. I came across this issue and after reading it carefully I thought I would try to reproduce it, and I was able to reproduce it."
- Right: "Hi, I reproduced this. Steps and output below."

### Rule: Calm tone

Report what happened with no hype. No exclamation points, no "amazing" or
"super excited".

- Wrong: "Super excited to jump on this!! Can't wait to dig in!"
- Right: "I'd like to work on this. I'll start by finding where the prompt opens."

### Rule: Promise the investigation only

When I claim an issue, I say what I'll look at and that a report is
coming. I never promise a fix, a PR, or a date.

- Wrong: "I'll have a fix up by Friday."
- Right: "I'll check the README against `.env.example` and post a repro report here."

### Rule: No "do X, not Y" sentences

State what is true or what I'll do. Leave out the contrast with what
I'm not doing.

- Wrong: "This is a bug in the decoder, not in the input."
- Right: "The panic comes from the decoder at decoder_hcl.go:341."

## Things I never post

- Begging or pleading: "please fix this", "please let me have this one",
  "🙏", "I really need this merged".
- Emojis of any kind.
- Em dashes. I use a period or a comma, or split the sentence in two.
- Promises I can't back up, like "I'll have a fix up tomorrow".
