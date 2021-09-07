# perfSONAR Web Site

These are the files used to generate the perfSONAR web site on GitHub pages.

The site is based on [Jekyll](https://jekyllrb.com), a static site
generator based on Ruby and the
[Liquid](https://shopify.github.io/liquid) templating language.

The recommended environment is the development VM described below.

## Development VM

### Prerequisites

 * Linux or OS X host system VirtualBox
 * Vagrant
 * The `vagrant-vbguest` plugin (installed with `vagrant plugin install vagrant-vbguest`).


### Setup

This process will build a VirtualBox VM with an account matching your
account on the host and your home directory available as a shared
folder.

Begin by cloning a copy of the site sources into your home directory.

Then build the VM:

```
you@host$ cd ~/path/to/checked/out/website
you@host$ make vm
```

Once the VM is built, log into it:
```
you@host$ make ssh
```


### Developing On the VM

Files in the cloned copy of the site can be edited on the host or the guest.

To build the site:

```
you@ps-site-dev$ cd ~/path/to/checked/out/website
you@ps-site-dev$ make build
jekyll build
Configuration file: /home/you/path/to/checked/out/website/_config.yml
            Source: /home/you/path/to/checked/out/website
       Destination: /home/you/path/to/checked/out/website/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
   GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
                    done in 4.162 seconds.
 Auto-regeneration: disabled. Use --watch to enable.

```

Alternately, a process can be started in another terminal to
continuously update the site as changes are made:

```
you@host$ make ssh
you@ps-site-dev$ cd ~/path/to/checked/out/website
you@ps-site-dev$ make dev
jekyll build --incremental --watch
Configuration file: /home/you/path/to/checked/out/website/_config.yml
            Source: /home/you/path/to/checked/out/website
       Destination: /home/you/path/to/checked/out/website/_site
 Incremental build: enabled
      Generating... 
   GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
                    done in 4.032 seconds.
 Auto-regeneration: enabled for '/home/you/path/to/checked/out/website'
```


To view the website, point a browser on the host at
`file:///home/you/path/to/checked/out/website/_site/index.html`.

## Docker image

Alternatively to the VM, you can use Docker to build and launch a container running the website.  This can be done with the following commands:

```
docker compose build
docker compose up
```

Once all the Gems are installed and Jekyll started, you should have the website running locally at http://0.0.0.0:4000/ or http://localhost:4000/  Any change in to the website files contained in the repository will be reflected on this live website.

### Updating the Gemfile.lock

In case the installation complains about failed dependencies or vulnerabilities in the Gems insalled and Jekyll doesn't start, you can run a shell in the running container to resolve that:

```
docker compose run --entrypoint=bash server
```

After that you can run `bundle update github-pages` or whatever other command Ruby Gems tells you to run and then you should end up with an updated Gemfile.lock with correct dependencies and running `docker compose up` again should bring the website alive.


## Other Notes

### News Items

Items for the news feed should be added to the `_posts` directory.  Use one
of the existing posts as a template.

Note that if the date for a news item is in the future, the site must
be rebuilt on or after the day the it should appear.  The easiest
thing to do is to push the change on the day it should appear.


### History

Items for history are in the `_history` collection.  Each item is in a
file named `YYYY-MM-DD-item.md` and contains a single field called
`text` that contains the item.

**NOTE:** The dates on history items receive special treatment for
  approximate or unknown dates:

  * January 1 refers to an entire year (e.g., 2005-01-01 really means 2005).
  * The first day of a month refers to the entire month (e.g., 2005-07-01 means July, 2005).


### Releases

Release notes are in the `_releasenotes` collection.  When adding a
new release, use an existing file as a template.

Things to do when releasing a new version:

 * Go through older releases and set the `supported` item to `false`
   in those that are no longer supported.  (Do this for betas, even if
   they were for a now-supported version.)

 * Add the release to the `local` section of `pages/about/history.md`,
   making sure to keep it in descending date order.


Note that if the date for a release is in the future, the site must be
rebuilt on or after the day the it should appear.  The easiest thing
to do is to push the change on the day it should appear.


### Favicon

The `images/favicon` and favicon data in `_include/head.html` were
generated with https://realfavicongenerator.net and a tightly-cropped
version of the logo.


### Redirects

The `redirects` directory contains a set of generated pages that use
HTML tags to redirect pages from the old web site to their
counterparts in the new one.  After the site is initially fielded,
there should be no need to make any changes to files in this
directory.

