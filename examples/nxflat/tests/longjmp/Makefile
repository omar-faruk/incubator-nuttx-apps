############################################################################
# examples/nxflat/tests/longjmp/Makefile
#
#   Copyright (C) 2009 Gregory Nutt. All rights reserved.
#   Author: Gregory Nutt <gnutt@nuttx.org>
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#
# 1. Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
# 2. Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in
#    the documentation and/or other materials provided with the
#    distribution.
# 3. Neither the name NuttX nor the names of its contributors may be
#    used to endorse or promote products derived from this software
#    without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS
# FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
# COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
# INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,
# BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS
# OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED
# AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
# LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN
# ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.
#
############################################################################

include $(TOPDIR)/Make.defs

BIN			= longjmp

R1SRCS			= $(BIN).c
R1OBJS			= $(R1SRCS:.c=.o)

R2SRC			= $(BIN)-thunk.S
R2OBJ			= $(R2SRC:.S=.o)

all: $(BIN)

$(R1OBJS): %.o: %.c
	@echo "CC: $<"
	$(Q) $(CC) -c $(CPICFLAGS) $< -o $@

$(R2OBJ): %.o: %.S
	@echo "AS: $<"
	$(Q) $(CC) -c $(CPICFLAGS) $< -o $@

$(BIN).r1: $(R1OBJS)
	@echo "LD: $<"
	$(Q) $(LD) $(NXFLATLDFLAGS1) -o $@ $^

$(R2SRC): $(BIN).r1
	@echo "MK: $<"
	$(Q) $(MKNXFLAT) -o $@ $^

$(BIN).r2: $(R2OBJ)
	@echo "LD: $<"
	$(Q) $(LD) $(NXFLATLDFLAGS2) -o $@ $(R1OBJS) $(R2OBJ)

$(BIN): $(BIN).r2
	@echo "LD: $<"
	$(Q) $(LDNXFLAT) $(LDNXFLATFLAGS) -o $@ $^

$(ROMFS_DIR)/$(BIN): $(BIN)
	$(Q) install -D $(BIN) $(ROMFS_DIR)/$(BIN)

install: $(ROMFS_DIR)/$(BIN)

clean:
	$(call DELFILE, $(BIN))
	$(call DELFILE, $(R2SRC))
	$(call DELFILE, *.r1)
	$(call DELFILE, *.r2)
	$(call CLEAN)
