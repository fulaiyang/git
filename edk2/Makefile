SHELL=/bin/bash
BUILDTARGET = RELEASE
NAME := edk_ovmf
VERSION := 0.0.0
RELEASE := 000

EDK_DIR = ${CURDIR}

JOBS := $(shell grep -c ^processor /proc/cpuinfo 2>/dev/null)
ifeq ($(JOBS),)
JOBS := 1
endif
ifeq ($(CSM),ENABLE)
CSM_CON := CSM_ENABLE
else
CSM_CON := NULL
endif

all: ovmf
edk_basetool:
	cd ${EDK_DIR} && \
        make -j$(JOBS) -C BaseTools -f GNUmakefile && \
        source ${EDK_DIR}/edksetup.sh

ovmf: edk_basetool
	${EDK_DIR}/OvmfPkg/build.sh \
       -a X64 \
       -D FD_SIZE_2MB \
       -D DEBUG_ON_SERIAL_PORT \
       -D $(CSM_CON) \
       -n $(JOBS) \
       -b $(BUILDTARGET)
clean:
	-cd ${EDK_DIR}/BaseTools && $(MAKE) clean -f GNUmakefile
	@rm -rf ${EDK_DIR}/Build
	cd ${EDK_DIR}/Conf && rm -rf `ls | grep -v "ReadMe.txt"` .cache .AutoGenIdFile.txt
 
.PHONY: all clean ovmf edk_basetool
