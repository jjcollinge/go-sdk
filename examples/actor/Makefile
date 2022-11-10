DOCKER_USER ?= dotjson
REDIS_PASSWD ?= "password123"

.PHONY: build
build:
	go build -o dist/client ./client/main.go
	go build -o dist/serving ./serving/main.go
	docker build -t "$(DOCKER_USER)/actor-client" dist/client -f ./Dockerfile
	docker build -t "$(DOCKER_USER)/actor-serving" dist/serving -f ./Dockerfile

.PHONY: push
push:
	docker push "$(DOCKER_USER)/actor-client"
	docker push "$(DOCKER_USER)/actor-serving"

.PHONY: redis
redis:
	helm upgrade --install redis bitnami/redis --set global.redis.password=$(REDIS_PASSWD) --wait

.PHONY: deploy
deploy: redis
	kubectl apply -f ./deploy