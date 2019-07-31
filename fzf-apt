#!/usr/bin/env bash
# fzf-apt allows to select apt packages by using fzf and
# builds commands based on the selected packages.
# Copyright (C) 2019  Nico Baeurer

# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU Affero General Public License as
# published by the Free Software Foundation, either version 3 of the
# License, or (at your option) any later version.

# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU Affero General Public License for more details.

# You should have received a copy of the GNU Affero General Public License
# along with this program.  If not, see <https://www.gnu.org/licenses/>.
function insert_stdin {
    # if this wouldn't be an external script
    # we could use 'print -z' in zsh to edit the line buffer
    stty -echo
    perl -e 'ioctl(STDIN, 0x5412, $_) for split "", join " ", @ARGV;' \
        "$@"
}


if [[ "${BASH_SOURCE[0]}" == "${0}" ]]; then
    packages="$(apt list --verbose 2>/dev/null | \
        # remove "Listing..."
        tail --lines +2 | \
        # place the description on the same line
        # separate the description and the other information
        # with a ^
        sd $'\n {2}([^ ])' $'^$1' | \
        # place the package information and the package description
        # in a table with two columns
        column -t -s^ | \
        # select packages with fzf
        fzf --multi | \
        # remove everything except the package name
        cut --delimiter '/' --fields 1 | \
        # escape selected packages (to avoid unwanted code execution)
        # and remove line breaks
        xargs --max-args 1 --no-run-if-empty printf "%q ")"

    if [[ -n "${packages}" ]]; then
        insert_stdin "# apt ${@}" "${packages}"
    fi
fi
