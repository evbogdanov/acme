#!/usr/bin/env bash

# Select `b` (or before it, or after) and assign Acme `addr` to selection
echo addr=dot | 9p write acme/$winid/ctl

# Mark the current line and change `addr` to it
echo -n '__MARKER__' | 9p write acme/$winid/data
echo -n '/^.*__MARKER__/' | 9p write acme/$winid/addr

# Process the line
line=$(9p read acme/$winid/data | sed 1q)
indentation=$(echo "$line" | sed -E 's/^(	*).*$/\1/')
before_marker=$(echo "$line" | sed -E 's/b?__MARKER__.*$/{/')
after_marker=$(echo "$line" | awk -F'__MARKER__' '{print $2}')
[ "$after_marker" == "b" ] && after_marker=""

# Compose the final block
block="$before_marker"$'\n'"$indentation	"$'\n'"$indentation}$after_marker"

# Change `addr` one more time and replace original line with my block
echo -n '/^.*__MARKER__.*\n?/' | 9p write acme/$winid/addr
echo "$block" | 9p write acme/$winid/data
