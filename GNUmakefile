TAG=tomgidden/ubuntu-server
USER?=me
CONTAINER?=ubuntu-server

# From https://github.com/solita/docker-systemd/issues/1#issuecomment-218190399
TWEAKS=--cap-add SYS_ADMIN --security-opt seccomp=unconfined --stop-signal=SIGRTMIN+3 --tmpfs /run --tmpfs /run/lock -v /sys/fs/cgroup:/sys/fs/cgroup:ro

KEYFILE=home/$(USER)/.ssh/authorized_keys

all:

clean:
	-docker kill $(CONTAINER)
	-docker rm $(CONTAINER)

distclean: clean
	-docker rmi $(TAG)

copy_ssh:
	cp ~/.ssh/id_rsa.pub $(KEYFILE)
#	cp ~/.ssh/authorized_keys $(KEYFILE)
#	cp ~/.ssh/id_ed25519.pub $(KEYFILE)
#	cp ~/.ssh/id_ed25519_docker.pub $(KEYFILE)

build: Dockerfile debconf.txt
	docker build . --build-arg user=$(USER) -t $(TAG)

run:
	docker run --rm --name $(CONTAINER) $(TWEAKS) -d -p 8022:22 -v `pwd`/home:/home $(TAG)

shell:
	docker exec -it -u $(USER) $(CONTAINER) zsh

ssh:
	ssh -p 8022 me@localhost
