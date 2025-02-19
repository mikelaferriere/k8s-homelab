# https://github.com/assafelovic/gpt-researcher

# Docker image
The "official" gpt-researcher docker image doesnt work on amd64, so this elestio image
should stay in sync with the repo's releases for the most part, so we'll use this

https://hub.docker.com/r/elestio/gpt-researcher

# Configs
the configs are not well documented. I needed to sift through this configs directory in order to determine what configs
I needed to set in order to get searxng and ollama to work correctly

https://github.com/assafelovic/gpt-researcher/blob/master/gpt_researcher/config/variables/default.py