

help:
	cat Makefile

# environments
chmod:
	docker-compose exec app chmod -R a+w storage
	docker-compose exec app chmod -R a+w bootstrap/cache
local:
	\cp -p .env.local .env

upmod:
	@make up
	@make chmod

# up/down
up:
	docker-compose up -d
upb:
	docker-compose up -d --build
upa:
	docker-compose -f ./docker-compose_apache.yml up -d
down:
	docker-compose down --remove-orphans
prune:
	docker system prune --volumes
restart:
	@make down
	@make up

# exec
app:
	docker-compose exec app ash
