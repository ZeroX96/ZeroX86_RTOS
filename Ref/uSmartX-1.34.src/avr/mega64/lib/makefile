# Makefile for AVR target

# Define directories.	
	uSMARTX_INC_DIR = ../../../inc
	uSMARTX_SRC_DIR = ../../../src/
	uSMARTX_HAL_INC_DIR = ../inc
	
# Define programs.
	AR = avr-ar
    CC = avr-gcc
    ASSEMBLE = avr-gcc -x assembler-with-cpp
    REMOVE = rm -f
    COPY = cp
    MOVE = mv

# MCU name
	MCU = atmega64

# Output format. Can be [srec|ihex].
    FORMAT = ihex

# Target file name (without extension).
	TARGET = libusmartx

# List C source files here.
	SRC =	$(uSMARTX_SRC_DIR)usmartx.c \
			$(uSMARTX_SRC_DIR)queue.c \
			$(uSMARTX_SRC_DIR)mailbox.c \
			$(uSMARTX_SRC_DIR)semaphore.c \
			$(uSMARTX_SRC_DIR)mballoc.c \
			$(uSMARTX_SRC_DIR)cycbuff.c			
					
# List Assembler source files here.
	ASRC =	../../hal/hal.s

# Compiler flags.
	CPFLAGS = -c -O2 -Wall
	
# Assembler flags.
    ASFLAGS = -Wa,-ahlms=$(<:.s=.lst)

# Linker flags (passed via GCC).
	LDFLAGS = 

# Additional library flags (-lm = math library).
	LIBFLAGS =

# Define all project specific object files.
	OBJ	= $(SRC:.c=.o) $(ASRC:.s=.o) 
	
# Define all listing files.
	LST = $(ASRC:.s=.lst) $(SRC:.c=.lst)
	
# Compiler flags to generate dependency files.
	GENDEPFLAGS = -Wp,-M,-MP,-MT,$(*F).o,-MF,.dep/$(@F).d

# Add target processor to flags.
   	CPFLAGS += -mmcu=$(MCU) $(GENDEPFLAGS)
	ASFLAGS += -mmcu=$(MCU)
	LDFLAGS += -mmcu=$(MCU)
  
# Default target.
.PHONY : Lib
Lib: Clean $(TARGET).a clear_output

# Link: create ELF output file from object files.
.SECONDARY : $(TRG).elf
.PRECIOUS : $(OBJ)
%.a: $(OBJ)
	$(AR) rs $(TARGET).a $(OBJ)

# Compile: create object files from C source files.
%.o : %.c
	$(CC) -c $(CPFLAGS) -I$(uSMARTX_INC_DIR) -I$(uSMARTX_HAL_INC_DIR) $< -o $@ 

# Assemble: create object files from assembler files.
%.o : %.s
	$(ASSEMBLE) -c $(ASFLAGS) $< -o $@

# Target: clean project.
.SILENT : Clean
.PHONY : Clean
Clean :
	$(REMOVE) $(TARGET).hex
	$(REMOVE) $(TARGET).obj
	$(REMOVE) $(TARGET).elf
	$(REMOVE) $(TARGET).bin
	$(REMOVE) $(TARGET).map
	$(REMOVE) $(TARGET).obj
	$(REMOVE) $(TARGET).sym
	$(REMOVE) $(TARGET).lnk
	$(REMOVE) $(TARGET).lss
	$(REMOVE) $(OBJ)
	$(REMOVE) $(LST)
	$(REMOVE) $(SRC:.c=.s)

# Target copy output files
.SILENT : move_output
.PHONY: move_output
move_output :
	$(MOVE) $(TARGET).a ../lib/$(TARGET).a
	
# Target clear output files
.SILENT : clear_output
.PHONY: clear_output
clear_output :
	$(RM)  $(uSMARTX_SRC_DIR)*.o
	$(RM)  *.o
	$(RM)  ../../hal/*.o
	$(RM)  ../../hal/*.lst
		
# Include the dependency files.
-include $(shell mkdir .dep 2>/dev/null) $(wildcard .dep/*)


# List assembly only source file dependencies here:

