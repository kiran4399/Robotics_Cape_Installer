TARGET = robotics_cape.so

SUBDIRS = $(shell find * -type d -print)
CC = gcc
LINKER   := gcc
TOUCH 	 := $(shell touch *)
CFLAGS := -Wall -fsingle-precision-constant -fPIC
LFLAGS	:= -lm -lrt -lpthread -shared -lprussdrvd

# add -DI2C_DEBUG for debugging
DEFS = -DEMPL_TARGET_LINUX -DMPU9150 -DAK8975_SECONDARY

SOURCES := $(shell find ./ -name '*.c')
INCLUDES := $(shell find ./ -name '*.h')
OBJECTS  := $(SOURCES:$%.c=$%.o)
RM := rm -f
CP := cp
INSTALL_DIR = /usr/lib/
INCLUDE_DIR = /usr/include

all:$(TARGET)
	
	
# linking Objects
$(TARGET): $(OBJECTS)
	@$(LINKER) $(LFLAGS) -o $(TARGET) $(OBJECTS)
	@echo
	@echo "Linking Complete"
	
$(OBJECTS): %.o : %.c
	@$(TOUCH) $(CC) $(CFLAGS) $(DEFS) -c $< -o $(@)
	@echo "Compiled "$<" successfully!"




# install to /usr/bin
$(phony all) : $(TARGET)
.PHONY: install

install:
	@echo moving headers
	@for dir in $(SUBDIRS) ; do \
		mkdir -p $(INCLUDE_DIR)/$$dir  ;  \
		install -m 0755 $$dir/*.h $(INCLUDE_DIR)/$$dir/ ; \
	done
	@install -m 0755 *.h $(INCLUDE_DIR)
	
	$(MAKE)
	@install -m 0755 $(TARGET) $(INSTALL_DIR)
	@install -m 0755 $(OBJECTS) $(INSTALL_DIR)
	#@install -m 0755 pru/libprussdrv.so $(INSTALL_DIR)
	@cp pru/prussdrv.h $(INCLUDE_DIR)
	@cp pru/pruss_intc_mapping.h $(INCLUDE_DIR)
	@cp -r mavlink/ $(INCLUDE_DIR)
	@cp $(INSTALL_DIR)/$(TARGET) $(INSTALL_DIR)/lib$(TARGET)
	@ldconfig
	@echo "$(TARGET) installed to $(INSTALL_DIR)"
	@echo
	

clean:
	@for dir in $(SUBDIRS) ; do \
		$(RM) $$dir/*.o ; \
	done
	
	@$(RM) *.o
	@$(RM) *.so
	@echo "Cleanup complete!"
