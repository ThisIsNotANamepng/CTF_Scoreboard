# Blugold Group Website

This website is the website for the Blugold Group, the cybersecurity club of the University of Wisconsin - Eau Claire.

It is not meant to be hosted on the open web, but rather on the cybersecurity lab network. Its hosted on the open web over the winterim 2024-2025 to provide access for members to a Winterim CTF

As we are a technical club, we try to make the documentation for the server as clear and comprehensive as possible to lower the barrier for members to dig into the code with as little tachnical experience as possible. As such, this documentation file aims to be a comprehensive guide to every moving part of the server.

This guide is still a work in progress

## Architecture

The server is written in python to allow a lower barrier of entry to developing it. By design, it should allow people to develop it long after I'm gone. That's why I'm trying to write this guide to be as comprehensive as possible.

### Routes

### Badges

To change: `UPDATE users SET tags = 'Exec,Master Locksmith' WHERE username = 'adminn';`

Has to be a comma seperated list, no spaces

Add new badges in the `/about/badges` page and the css of `/members`

### Database

## Hall of History

If you add on to this codebase, please add your name below

This server was created by Jack Hagen (the founder of the club) over a weekend in October 2024

### Historical Notes

The CLEAS was originally meant to authenticate id tags unique to each club member, allowing us to see who unlocked what when. The lock permissions were meant to be tied to user accounts on this server, so any mention of locks in the code references that. Ultimately we just used a basic nfc lock which uses one nfc password which was copied to several tags to be shared by all members.

## Server Setup

At the time of writing, I am currently setting up the server to run on a raspberry pi, using Cloudflare to tunnel to the open internet. I don't know how you will host this in the future, but as of now this file will hold documentation for how the server is setup as well as the programming

For some reason when running from cron after reboots, setting up a Cloudflare tunnel and SSH tunnel fail because they can't reach the network, I'm assuming that network services just haven't booted by that point, so delaying them 10 seconds did the trick  

To start the server (**in debug mode**), first ensure Python and Pip are installed.
Run `pip install -r requirements.txt` to install packages required for the server to run. Finally, run `python app.py`. The site should be accessible at any of the given addresses.

An initial admin user must be created. This can be done by directly modifying the new `database.db` file. Add a record to the `users` table. The password field should contain a hashed password, generated using [werkzeug.security.generate_password_hash()](https://werkzeug.palletsprojects.com/en/stable/utils/#werkzeug.security.generate_password_hash). A Python script can accomplish this, such as [this one](https://github.com/EekRats/ctf_hasher/blob/main/ctf_hasher.py). The `is_admin` column should be set to `1`.

## Important Files

Some files/directories of particular importance include:
* `app.py` -- Main file defining Flask `route()`s. These call functions when a client navigates to a specific URL. For our use case, this typically means returning `render_template()` that renders a .html file.
* `/templates` -- This directory contains all of the .html files referenced by various `route()`s.
* `helpers.py` -- Python script containing definitions for a variety of helper functions used elsewhere
* `config.py` -- Contains variable definitions for various configurable things. Add definitions here when at all possible so that the specifics of this site can be changed as needed in the future.
* `base.html` -- This contains the universal page header, navbar, logo, etc. In the child .html, you may define `{% block %}`s for `content`, `style`, and `title`. All pages should use `{% extends "base.html" %}` to inherit base.html. The `style` block can be used to set not only the style tag, but any other tags that must be included in the header such as `<script>`
