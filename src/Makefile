# Created by CuriousTorvald and Claude on 2025-08-17.
# Makefile for TSVM Enhanced Video (TEV) encoder

CC = gcc
CXX = g++
CFLAGS = -std=c99 -Wall -Wextra -Ofast -D_GNU_SOURCE
CXXFLAGS = -std=c++11 -Wall -Wextra -Ofast -D_GNU_SOURCE

# Zstd flags (use pkg-config if available, fallback for cross-platform compatibility)
ZSTD_CFLAGS = $(shell pkg-config --cflags libzstd 2>/dev/null || echo "")
ZSTD_LIBS = $(shell pkg-config --libs libzstd 2>/dev/null || echo "-lzstd")
LIBS = -lm $(ZSTD_LIBS)

# Source files and targets
TAD_TARGETS = encoder_tad decoder_tad

# Build all encoders
all: $(TAD_TARGETS)

# Build TAD (Terrarum Advanced Audio) tools
encoder_tad: encoder_tad_standalone.c encoder_tad.c encoder_tad.h
	rm -f encoder_tad encoder_tad_standalone.o encoder_tad.o
	$(CC) $(CFLAGS) $(ZSTD_CFLAGS) -c encoder_tad.c -o encoder_tad.o
	$(CC) $(CFLAGS) $(ZSTD_CFLAGS) -c encoder_tad_standalone.c -o encoder_tad_standalone.o
	$(CC) -o encoder_tad encoder_tad_standalone.o encoder_tad.o $(LIBS)

decoder_tad: decoder_tad.c
	rm -f decoder_tad
	$(CC) $(CFLAGS) $(ZSTD_CFLAGS) -o decoder_tad $< $(LIBS)

# Build all TAD tools
tad: $(TAD_TARGETS)

# Build with debug symbols
debug: CFLAGS += -g -DDEBUG
debug: $(TARGETS)

# Clean build artifacts
clean:
	rm -f $(TAD_TARGETS) *.o

# Install (copy to PATH)
install: $(TAD_TARGETS)
	cp encoder_tad /usr/local/bin/
	cp decoder_tad /usr/local/bin/

# Check for required dependencies
check-deps:
	@echo "Checking dependencies..."
	@pkg-config --exists libzstd || (echo "Error: libzstd-dev not found. Install with: sudo apt install libzstd-dev" && exit 1)
	@echo "All dependencies found."

# Help
help:
	@echo "TSVM Advanced Video (TAV) and Audio (TAD) Encoders"
	@echo ""
	@echo "Targets:"
	@echo "  all          - Build video encoders (default)"
	@echo "  tad          - Build all TAD audio tools (encoder, decoder)"
	@echo "  encoder_tad  - Build TAD audio encoder"
	@echo "  decoder_tad  - Build TAD audio decoder"
	@echo "  debug        - Build with debug symbols"
	@echo "  clean        - Remove build artifacts"
	@echo "  install      - Install to /usr/local/bin"
	@echo "  check-deps   - Check for required dependencies"
	@echo "  help         - Show this help"
	@echo ""
	@echo "Usage:"
	@echo "  make               # Build video encoders"
	@echo "  make tad           # Build all TAD audio tools"
	@echo "  sudo make install  # Install all encoders"

.PHONY: all clean install check-deps help debug tad
