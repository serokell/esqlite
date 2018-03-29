CWD := $(shell pwd)
ROOT := $(abspath $(CWD)/..)

ERTS_INCLUDE_DIR ?= $(shell erl -noshell -s init stop -eval "io:format(\"~s/erts-~s/include/\", [code:root_dir(), erlang:system_info(version)]).")
ERL_INTERFACE_INCLUDE_DIR ?= $(shell erl -noshell -s init stop -eval "io:format(\"~s\", [code:lib_dir(erl_interface, include)]).")
ERL_INTERFACE_LIB_DIR ?= $(shell erl -noshell -s init stop -eval "io:format(\"~s\", [code:lib_dir(erl_interface, lib)]).")

CC ?= cc
CFLAGS += -fPIC -I $(ERTS_INCLUDE_DIR) -I $(ERL_INTERFACE_INCLUDE_DIR) -DSQLITE_THREADSAFE=1 -DSQLITE_USE_URI -DSQLITE_ENABLE_FTS3 -DSQLITE_ENABLE_FTS3_PARENTHESIS

LDFLAGS += -shared
LDLIBS += -L $(ERL_INTERFACE_LIB_DIR) -lerl_interface -lei

C_SRC_DIR = $(CWD)
C_SRC_OUT ?= $(ROOT)/priv/esqlite3_nif.so

SOURCES := $(shell find $(C_SRC_DIR) -type f -name "*.c")
OBJECTS = $(addsuffix .o, $(basename $(SOURCES)))

$(C_SRC_OUT): $(OBJECTS)
	mkdir -p $(ROOT)/priv/
	$(CC) $(OBJECTS) $(LDFLAGS) $(LDLIBS) -o $(C_SRC_OUT)

%.o: %.c
	$(CC) $(CFLAGS) $(CPPFLAGS) -c $(OUTPUT_OPTION) $<
