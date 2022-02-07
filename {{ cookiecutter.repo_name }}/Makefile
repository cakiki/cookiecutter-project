VERSION := 0.0.1
NAME := {{ cookiecutter.project_name }}
REPO := {{ cookiecutter.author_name }}

build-gpu: clean
	docker build -f dockerfiles/Dockerfile.tf.gpu -t ${REPO}/${NAME}:${VERSION} -t ${REPO}/${NAME}-gpu:latest .

build-cpu: clean
	docker build -f dockerfiles/Dockerfile.cpu -t ${REPO}/${NAME}-cpu:${VERSION} -t ${REPO}/${NAME}-cpu:latest .

run-gpu: build
	docker run --rm -it -p 8888:8888 -p 6006:6006 --gpus all --env PYTHONPATH=/tf/src --mount type=bind,source=${PWD},target=/tf ${REPO}/${NAME}-gpu:${VERSION} && make -s clean

run-cpu: build-cpu
	docker run --rm -it --network host --env PYTHONPATH=/tf/src --mount type=bind,source=${PWD},target=/home/jovyan/work --workdir=/home/jovyan/work ${REPO}/${NAME}-cpu:${VERSION} jupyter notebook

elastic:
	docker run --rm -it --mount type=bind,source=${PWD}/data/ES,target=/usr/share/elasticsearch/data \
		--network host --memory=16g --env discovery.type=single-node \
		docker.elastic.co/elasticsearch/elasticsearch:7.17.0

clean:
	sudo chown -R 1000:1000 .