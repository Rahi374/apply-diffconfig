# apply-diffconfig
Script to apply Linux kernel diffconfig onto config

We have `scripts/diffconfig` (in the kernel source) to generate diffs
between configs.

The problem was that I couldn't find a nice and easy way to apply the
diffs onto a config. They weren't standard diffs that could be handled by
`git` or `patch`. I wanted to be able to make a few changes to the diff and
still be able to apply it.

So I set out to write my own script to do it. Ideally it would've been in perl,
but I'm not fluent enough in it, so it's in ruby for now.


# Usage

`apply-diffconfig DIFF OLD_CONFIG`

Output is to stdout, so redirect if you want the output to go to a file:

`apply-diffconfig DIFF OLD_CONFIG > NEW_CONFIG`

# diffconfig

In case this is a new discovery for you too, `diffconfig` lives in the kernel
source at `scripts/diffconfig`. You feed it two configs and it spits out the
diff between them; more precisely, it is what changed from the first config
to the second config.

Each line is either of the form `-OPTION [y|n|m|int|text]`, `+OPTION [y|n|m|int|text]`, or
` OPTION [y|n|m|int|text] -> [y|n|m|int|text]` (note the preceding space -- thank you markdown for
removing it). It makes it
very easy to be read by humans, but a lack of line numbers means that neither `git` nor `patch`
can handle it (as far as I know) if you want t "apply" the diff in the same manner as a patch.

It is also unfortunately tedious to go through your config and edit each option by hand.
Its worse when there are dependencies. If you can just apply the whole diff, then there would
be no problem, and that is my rationale for writing this.
