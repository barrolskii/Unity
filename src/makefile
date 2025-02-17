CC := gcc
CFLAGS := -Wall -Wextra -pedantic -ggdb
EXE := prog

BUILD_DIR := ./build
SRC_DIR ?= ./

# Find all source files we want to compile
SRC := $(shell find $(SRC_DIR) -name '*.c')

# String substitution for the source files eg. hello.c -> hello.c.o
OBJS := $(SRC:%=$(BUILD_DIR)/%.o)

# String substitution eg. hello.c.o -> hello.c.d
DEPS := $(OBJS:.o=.d)

INC_DIRS := $(shell find $(SRC_DIR) -type d)
INC_FLAGS := $(addprefix -I,$(INC_DIRS))

# Set the preprocessor flags to generate makefiles for us
CPPFLAGS=$(INC_FLAGS) -MMD -MP

# Final build step to make debug executable
$(EXE): $(OBJS)
	$(CC) $(OBJS) -o $@ $(LDFLAGS)

$(BUILD_DIR)/%.c.o: %.c
	mkdir -p $(dir $@)
	$(CC) $(CPPFLAGS) $(CFLAGS) -c $< -o $@


.PHONY: clean
clean:
	rm -rf $(EXE) $(BUILD_DIR)

# Include the .d makefiles
# The '-' prefix supresses errors when the makefiles are missing
# Initial runs of this will always have the makefiles missing so we don't
# want these errors to be reported
-include $(DEPS)
