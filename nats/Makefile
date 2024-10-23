.DEFAULT_GOAL := up

DF := docker/docker-compose.yml

up:
	docker compose -f ${DF} up -d

down:
	docker compose -f ${DF} down

stop:
	docker compose -f ${DF} stop nats

start:
	docker compose -f ${DF} start nats
