---
date: 2019-01-03
---
<meta property="og:published_time" content="2019-01-03">

# Handy Makefile Rules and Patterns

## Patterns

Required variable

```makefile
$(or $(build),$(error Must set build variable))
```

Optional variable with fallback

```makefile
$(or $(branch),master)
```

## Git

Rebase on origin’s master

```makefile
rebase_origin_master:
	git pull --rebase origin master
```

Create a git commit message template with co-authored-by set

```makefile
# Requires card=…, name=…, and a contributors.txt file
pair:
	@echo "$(card) \n\nCo-authored-by: $(shell grep -i $(name) contributors.txt)" > .git/.gitmessage
	@git config commit.template .git/.gitmessage
	@cat .git/.gitmessage
```

## Rails

Run the most recently modified spec

```makefile
recent_spec:
	bundle exec rspec $(or $(shell find ./spec -name "*_spec.rb" -cmin -5 -exec ls -1tr "{}" + | tail -n 1),$(error No *_spec.rb file modified recently))
```

Run specs with changes not yet committed

```makefile
uncommitted_specs:
	bundle exec rspec $(shell git status --porcelain | grep -E "^[^D][^D] .+_spec.rb$" | cut -c4-)
```
