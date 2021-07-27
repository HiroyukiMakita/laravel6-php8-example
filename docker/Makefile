#########################################################
# imports
#########################################################

include .env
PHP_PROJECT_ROOT := ${PHP_PROJECT_ROOT}

#########################################################
# variables
#########################################################

COMPOSE := docker-compose
BUILD := $(COMPOSE) build --no-cache
UP := $(COMPOSE) up -d
DOWN := $(COMPOSE) down
EXEC = $(COMPOSE) exec $(CONTAINER) bash $(ARG)
CONTAINER =
ARG =

# docker services (container name)
PHP := php

#########################################################
# make commands
#########################################################

build:
	@$(BUILD)

laravel:
	@bash laravel-installer.sh

laravel-exec-php:
	$(eval CONTAINER := $(PHP))
	$(eval ARG := -c "\
	cd $(PHP_PROJECT_ROOT)/docker && \
	bash laravel-installer.sh\
	")
	@$(EXEC)

up:
	@$(UP)

exec-php:
	$(eval CONTAINER := $(PHP))
	$(eval ARG := -c "cd $(PHP_PROJECT_ROOT) && bash")
	@$(EXEC)

down:
	@$(DOWN)

remove:
	@$(DOWN) --rmi all --volumes --remove-orphans
