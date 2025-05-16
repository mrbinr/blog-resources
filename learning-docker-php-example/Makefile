ifneq (,$(wildcard ./.env.docker))
    include .env.docker
    export
endif

ifeq (, $(which -v docker-compose))
	DOCKER_COMPOSE = docker compose -f docker-compose-dev.yml
else
	DOCKER_COMPOSE = docker-compose -f docker-compose-dev.yml
endif

MAKEFLAGS += --always-make
ARGS ?= $(strip $(subst ',\\',$(subst ",\\",$(wordlist 2,$(words $(MAKECMDGOALS)),$(MAKECMDGOALS)))))

# Misc
.DEFAULT_GOAL = help
.PHONY    = help up down logs composer php sh console cc

## -- Docker Node Makefile --
help: ## Outputs this help screen
	@grep -E '(^[a-zA-Z0-9_-]+:.*?##.*$$)|(^##)' $(firstword $(MAKEFILE_LIST)) | \
		awk 'BEGIN {FS = ":.*?## "}{printf "\033[32m%-30s\033[0m %s\n", $$1, $$2}'

## -- Docker containers --
.PHONY = up
up: ## Build the images and start the containers
	@$(DOCKER_COMPOSE) up -d --build

.PHONY = down
down: ## Stop the docker hub
	@$(DOCKER_COMPOSE) down --remove-orphans

.PHONY = start
start: ## Start containers
	@$(DOCKER_COMPOSE) start

.PHONY = stop
stop: ## Stop containers
	@$(DOCKER_COMPOSE) stop

## -- Composer --
.PHONY = composer
composer: ## Run composer
	@docker exec -ti $(CONTAINER_PHP) composer $(ARGS)

## -- PHP --
.PHONY = php
php: ## Run php command line
	@docker exec -ti $(CONTAINER_PHP) php $(ARGS)

.PHONY = server-start
server-start: ## Start PHP server
	@docker exec -t $(CONTAINER_PHP) php -S 0.0.0.0:9000 -t public/ > /dev/null &

.PHONY = server-stop
server-stop: ## Stop PHP server
	@docker exec -t $(CONTAINER_PHP) bash -c "ps auxw | grep 'php -S' | \
		grep -v grep | awk '{print \$$1}' | xargs kill"

## -- Shell --
.PHONY = shell
shell: ## Connect to php container
	@docker exec -ti $(CONTAINER_PHP) bash

.PHONY = shell-root
shell-root: ## Connect to php container
	@docker exec -ti -u root $(CONTAINER_PHP) bash

## -- Logs --
.PHONY = logs
logs: ## Show live logs
	@$(DOCKER_COMPOSE) logs --tail=0 --follow

# Avoid to build argument as a target
%::
	@true
