include ../../make.rules

RUNC_REV := 82d2fa4eb069b98af7c34ae525f85fcfb0e40a22
RUNC_PATH := $(CURDIR)/../../../runc
EXTRA_LDFLAGS := "-X main.runqCommit=$(GIT_COMMIT)"

all: $(RUNC_PATH)/.git
	git -C $(RUNC_PATH) clean -fd
	git -C $(RUNC_PATH) checkout -q -f $(RUNC_REV)
	git -C $(RUNC_PATH) apply $(CURDIR)/runc.patch
	CC=gcc EXTRA_LDFLAGS=$(EXTRA_LDFLAGS) $(MAKE) -C $(RUNC_PATH) BUILDTAGS="" runc
	cp -f $(RUNC_PATH)/runc $(CURDIR)/runq

$(RUNC_PATH)/.git:
	@echo Error: RunC source not found
	@echo To download RunC source: '"git clone https://github.com/opencontainers/runc"'; false

install: runq
	install -m 0755 -D $(CURDIR)/runq $(RUNQ_ROOT)/runq

clean:
	cd $(RUNC_PATH) && git clean -xfd && git reset --hard && git checkout master
	rm -f runq
