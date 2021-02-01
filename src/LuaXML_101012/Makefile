# 2009-03-16 by gf
#
ifeq ($(PREFIX),)
    PREFIX := /usr
endif

# generic compiler and linker settings:
#CROSS_COMPILE=arm-openwrt-linux-uclibcgnueabi-
#CC=$(CROSS_COMPILE)gcc

INCDIR ?= -I../lua/src -I $(STAGING_DIR)/usr/include
LIB    ?= 
LIBDIR ?=  -L. -L../lua/src -L $(STAGING_DIR)/usr/lib
override CFLAGS ?= -Os -Wall -c #-g

# generic platform specific rules:
ARCH_OS = $(shell uname -s)
ifeq ($(ARCH_OS),Linux)
  override CFLAGS += -fPIC
  override LFLAGS += -fPIC -shared
  LIBS          = $(LIBDIR) $(LIB) #-llua -ldl
  EXESUFFIX =
  SHLIBSUFFIX = .so

else  
  ifeq ($(ARCH_OS),Darwin) # MacOSX
    LFLAGS = -bundle 
    LIBS          = $(LIBDIR) -L/usr/local/lib $(LIB) -llua
    EXESUFFIX = .app
    SHLIBSUFFIX = .so
    
  else  # windows, MinGW
    LFLAGS =  -shared
    LIBS          = $(LIBDIR) $(LIB) -llua51 -mconsole -s
    EXESUFFIX = .exe
    SHLIBSUFFIX = .dll

  endif
endif

# project specific targets:
all:  LuaXML_lib$(SHLIBSUFFIX)

# project specific link rules:
LuaXML_lib$(SHLIBSUFFIX): LuaXML_lib.o
	$(CC) -o $@ $(LFLAGS) $^ $(LIBS) 

# project specific dependencies:
LuaXML_lib.o:  LuaXML_lib.c

# generic rules and targets:
.c.o:
	$(CC) $(CFLAGS) $(INCDIR) -c $<

install:
	install -d $(DESTDIR)$(PREFIX)/lib/lua
	install -m 644 LuaXML_lib.so $(DESTDIR)$(PREFIX)/lib/lua
	install -m 755 LuaXml.lua    $(DESTDIR)$(PREFIX)/lib/lua

clean:
	rm -f *.o *~ LuaXML_lib.so LuaXML_lib.dll

