.PHONY:

help:
	cat Makefile

PRODUCT_NAME := sogaoh-app

IMG_TAG := latest
NODEV := --no-dev

BLDARG_TZ        := --build-arg TZ=Asia/Tokyo
BLDARG_PHP_MODE  := --build-arg PHP_MODE=production
BLDARG_NODEV_OPT := --build-arg NO_DEV_OPT=$(NODEV)

AWS_ACCOUNT_ID :=
AWS_REGION := ap-northeast-1


# environments
local:
	\cp -p .env.local .env


# build
build:
	docker build --no-cache -t $(PRODUCT_NAME)/lara-sample:$(IMG_TAG) -f ./ship/docker/php/Dockerfile $(BLDARG_TZ) $(BLDARG_PHP_MODE) $(BLDARG_NODEV_OPT) .
# tag
tag:
	docker tag $(PRODUCT_NAME)/lara-sample:$(IMG_TAG) "$(AWS_ACCOUNT_ID).dkr.ecr.$(AWS_REGION).amazonaws.com/$(PRODUCT_NAME)/lara-sample:$(IMG_TAG)"
# push
push:
	docker push "$(AWS_ACCOUNT_ID).dkr.ecr.$(AWS_REGION).amazonaws.com/$(PRODUCT_NAME)/lara-sample:$(IMG_TAG)"

# ecr login
ecr-login:
	#aws configure set aws_access_key_id ${AWS_ACCESS_KEY_ID} --profile ${AWS_DEFAULT_PROFILE}
	#aws configure set aws_secret_access_key ${AWS_SECRET_ACCESS_KEY} --profile ${AWS_DEFAULT_PROFILE}
	aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin $(AWS_ACCOUNT_ID).dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com




# up/down
up:
	docker-compose up -d
upb:
	docker-compose up -d --build
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


# logs
log-a:
	docker-compose logs -f app
