# Copyright (c) 2005-2016 Intel Corporation
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
#
#
#

# GNU Makefile that builds and runs example.
run_cmd=
PROG=stereo
ARGS=
PERF_RUN_ARGS=

# icpc by default
ifneq (,$(shell which icc 2>/dev/null))
CXX=icpc
endif

# autodetect C++11 support
include ../../common/examples-common.inc

# TBB libs
TBBLIB=-ltbb
TBBLIB_DEBUG=-ltbb_debug

# OpenCL lib
OPENCL_LIB=-lOpenCL

ifeq ($(shell uname), Linux)
ifeq ($(target), android)
LIBS+= --sysroot=$(SYSROOT)
run_cmd=../../common/android.linux.launcher.sh
else
LIBS+= -lrt
endif
else
ifeq ($(shell uname),Darwin)
override CXXFLAGS += -Wl,-rpath,$(TBBROOT)/lib
override OPENCL_LIB=-framework OpenCL
endif
endif

all: release test

release: *.cpp
	$(CXX) -O2 -DNDEBUG $(CXXFLAGS) -o $(PROG) $^ $(TBBLIB) $(OPENCL_LIB) $(LIBS) $(CXX0XFLAGS)

debug: *.cpp
	$(CXX) -O0 -g -DTBB_USE_DEBUG $(CXXFLAGS) -o $(PROG) $^ $(TBBLIB_DEBUG) $(OPENCL_LIB) $(LIBS) $(CXX0XFLAGS)

clean:
	$(RM) $(PROG) *.o *.d

test:
	$(run_cmd) ./$(PROG) $(ARGS)

perf_build: release

perf_run:
	$(run_cmd) ./$(PROG) $(PERF_RUN_ARGS)

