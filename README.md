# brianreumere.software

## Overview

This is a collection of roles that install particular pieces of software. See role documentation for details.

## Example

Install the collection:

```sh
ansible-galaxy collection install brianreumere.software
```

Or include it in a `requirements.yml` file:

```yaml
collections:
  - name: brianreumere.software
    version: ">=1.0.0,<2.0.0"
    source: https://galaxy.ansible.com
```

And install it:

```sh
ansible-galaxy collection install -r requirements.yml
```
