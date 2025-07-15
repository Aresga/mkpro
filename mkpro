#!/bin/bash

# --- Helper function for colored output ---
print_success() {
    echo -e "\033[0;32m$1\033[0m"
}

print_info() {
    echo -e "\033[0;34m$1\033[0m"
}

print_error() {
    echo -e "\033[0;31m$1\033[0m"
}


# --- Prompt for Project Details ---
read -p "Enter your project directory name: " PROJECT_DIR
read -p "Enter your executable name (e.g., my_app): " EXECUTABLE_NAME
read -p "Enter the compiler to use (e.g., c++, g++): " COMPILER

# Use defaults if inputs are empty
if [ -z "$PROJECT_DIR" ]; then
    PROJECT_DIR="my_cpp_project"
    print_info "Using default project directory: $PROJECT_DIR"
fi
if [ -z "$EXECUTABLE_NAME" ]; then
    EXECUTABLE_NAME="app"
    print_info "Using default executable name: $EXECUTABLE_NAME"
fi
if [ -z "$COMPILER" ]; then
    COMPILER="c++"
    print_info "Using default compiler: $COMPILER"
fi

# Exit if the directory already exists
if [ -d "$PROJECT_DIR" ]; then
    print_error "Directory '$PROJECT_DIR' already exists. Exiting."
    exit 1
fi


# --- Create Project Directory Structure ---
print_info "\nCreating project directory: $PROJECT_DIR"
mkdir -p "$PROJECT_DIR/src" "$PROJECT_DIR/inc"
cd "$PROJECT_DIR" || exit


# --- Generate Makefile ---
print_info "Generating Makefile..."

# Using  a 'here document' (cat << EOL) to write the Makefile content.
# We escape Makefile-specific variables (like $(NAME)) with a backslash (\$)
# so they are written literally into the file instead of being interpreted
# by the shell.
cat > Makefile << EOL
#COLORS
GREEN=\\033[0;32m
RED=\\033[0;31m
RESET=\\033[0m

# EXECUTABLE
NAME = ${EXECUTABLE_NAME}

# COMPILER
CC = ${COMPILER}
FLAGS = -Wall -Wextra -Werror -std=c++98

# DIRECTORIES
SRC_DIR = src/
OBJ_DIR = obj/
INC_DIR = inc/

# This is a placeholder. You will need to add your .cpp files here after later

SRC_FILES = main.cpp

# Generate full paths for sources and objects
SRCS = \$(addprefix \$(SRC_DIR), \$(SRC_FILES))
OBJS = \$(addprefix \$(OBJ_DIR), \$(SRC_FILES:.cpp=.o))

all: \$(NAME)

# Rule to build object files
\$(OBJ_DIR)%.o: \$(SRC_DIR)%.cpp
	@mkdir -p \$(OBJ_DIR)
	@echo "\$(GREEN)🛠️  Compiling $< \$(RESET)"
	@\$(CC) \$(FLAGS) -I\$(INC_DIR) -c \$< -o \$@

# Rule to link the final executable
\$(NAME): \$(OBJS)
	@echo "\$(GREEN)🔗 Linking \$(NAME) \$(RESET)"
	@\$(CC) \$(FLAGS) -I\$(INC_DIR) \$(OBJS) -o \$(NAME)

# Rule to clean objs 
clean:
	@echo "\$(RED)🧹 Removing object files... \$(RESET)"
	@rm -rf \$(OBJ_DIR)

# Rule to clean objs and executable
fclean: clean
	@echo "\$(RED)🧹 Removing executable: \$(NAME) \$(RESET)"
	@rm -f \$(NAME)

re: fclean all

.PHONY: all clean fclean re
EOL


# --- Generate src/main.cpp ---
print_info "Generating src/main.cpp..."
cat > src/main.cpp << EOL
#include <iostream>

int main()
{
    std::cout << "Hello, ${EXECUTABLE_NAME}!" << std::endl;
    return 0;
}
EOL


# --- Final Instructions ---
echo ""
print_success "✅ Project '$PROJECT_DIR' created successfully!"
print_info "Navigate to it using: cd $PROJECT_DIR"
print_info "Add your .cpp files to the SRC_FILES variable in the Makefile."

