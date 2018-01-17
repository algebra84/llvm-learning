# Specify the gcc executable you will use the dragonegg plugin with either here
# or on the command line, for example like this (replace gcc-4.7 with the gcc
# you want to load the plugin into):
#   GCC=gcc-4.7 make ...
# If you don't specify anything, then by default the plugin targets the compiler
# used to build it.
GCC?=$(CC)

# Specify the copy of LLVM you will build the plugin against by giving its
# llvm-config here or on the command line.  To use an installed copy of LLVM,
# specify the installed llvm-config (just 'llvm-config' is enough if it is in
# your path).  It is not necessary to install LLVM to build dragonegg against
# it.  Instead you can do an LLVM build and point LLVM_CONFIG to the copy of
# llvm-config that was created during the build.
LLVM_CONFIG?=llvm-config


SRC_DIR=$(PWD)

ifndef VERBOSE
QUIET:=@
endif


LDFLAGS+=$(shell $(LLVM_CONFIG) --ldflags)
COMMON_FLAGS=-Wall -Wextra
CXXFLAGS+=$(COMMON_FLAGS) $(shell $(LLVM_CONFIG) --cxxflags)
CPPFLAGS+=$(shell $(LLVM_CONFIG) --cppflags) -I$(SRC_DIR)



CLANGLIBS = \
	-Wl,--start-group\
	-lclang\
	-lclangFrontend\
	-lclangDriver\
	-lclangSerialization\
	-lclangParse\
	-lclangSema\
	-lclangAnalysis\
	-lclangEdit\
	-lclangAST\
	-lclangLex\
	-lclangBasic\
	-Wl,--end-group

LLVMLIBS=$(shell $(LLVM_CONFIG) --libs bitwriter core support)

PROJECT=myiremit

PROJECT_OBJECTS=ir-emit.o

default: $(PROJECT)

ir-emit.o : $(SRC_DIR)/ir-emit.cpp
		@echo Compiling $*.cpp
		$(QUIET)$(CXX) -c $(CPPFLAGS) $(CXXFLAGS) $<

$(PROJECT) : $(PROJECT_OBJECTS)
		@echo Linking $@
		$(QUIET)$(CXX) -o $@ $(CXXFLAGS) $(LDFLAGS) $^ $(CLANGLIBS) $(LLVMLIBS) -lpthread


clean::
		$(QUIET)rm -f $(PROJECT) $(PROJECT_OBJECTS)
