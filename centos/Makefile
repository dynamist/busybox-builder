build:
	docker build --rm -t busybox-centos .
	../scripts/copy-file-to-host busybox-centos busybox
	../scripts/copy-file-to-host busybox-alpine config
