# SPDX-License-Identifier: MIT
# Automake file for ruri
AUTOMAKE_OPTIONS = subdir-objects

bin_PROGRAMS = ruri

# Source files
ruri_SOURCES = src/caplist.c src/chroot.c src/cprintf.c \
                src/info.c src/rurienv.c src/rurifetch.c src/seccomp.c \
                src/signal.c src/umount.c src/unshare.c src/rootless.c \
                src/mount.c src/k2v.c src/elf-magic.c src/config.c \
                src/cgroup.c src/passwd.c src/ps.c src/easteregg/action.c \
                src/easteregg/nekofeng.c src/easteregg/layer.c \
                src/easteregg/typewriter.c src/ruri.c

# Compiler and linker flags
AM_CFLAGS = $(CFLAGS)
AM_LDFLAGS = $(LINKER_FLAGS)

if ENABLE_STRIP
strip_target:
	$(STRIP) $(bin_PROGRAMS)
else
strip_target:
	@echo "strip not found, skipping stripping."
endif

if ENABLE_UPX
upx_target:
	$(UPX) --best --lzma $(bin_PROGRAMS)
else
upx_target:
	@echo "UPX not found, skipping compression."
endif

if !ENABLE_DEBUG
install-data-hook: strip_target upx_target
endif

#Check VERBOSE vaule
AM_V = $(if $(VERBOSE),, @)

AM_V_CC = $(AM_V)echo "  CC       " $@;

# Custom commands
.PHONY: ruritest check

check:
	clang-tidy $(ruri_SOURCES) || true

testuite:
	$(MAKE) -C test -f Makefile.test test
