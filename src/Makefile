.RECIPEPREFIX += 
.PHONY: clean

# Output Path
OUT_PATH = ../build
# Buildroot Output Path
BR_OUT_PATH = ../../amiibrOS-buildroot/output
# Host Root File System
HOST_ROOTFS = $(BR_OUT_PATH)/host
# Target Root File System
TARG_ROOTFS = $(BR_OUT_PATH)/target
# Where to Install Headers for Development
H_INSTALL_PATH = ../../subproj/include

# Custom Compiler and Archiver from Buildroot
CC = $(HOST_ROOTFS)/bin/arm-linux-gcc
AR = $(HOST_ROOTFS)/bin/arm-linux-gcc-ar
# Define compiler flags:
#  -O1                      defines optimization level
#  -g                       include debug information on compilation
#  -s                       strip unnecessary data from build
#  -Wall                    turns on most, but not all, compiler warnings
#  -std=c99                 defines C language mode (standard C from 1999 revision)
#  -std=gnu99               defines C language mode (GNU C from 1999 revision)
#  -Wno-missing-braces      ignore invalid warning (GCC bug 53119)
#  -D_DEFAULT_SOURCE        use with -std=c99 on Linux and PLATFORM_WEB,
#    required for timespec
#  -Werror=pointer-arith    catch unportable code that does direct arithmetic
#    on void pointers
#  -fno-strict-aliasing     jar_xm.h does shady stuff (breaks strict aliasing)
CFLAGS += -O1 -Wall -std=c99 -D_DEFAULT_SOURCE -Wno-missing-braces \
  -Werror=pointer-arith -fno-strict-aliasing -fPIC -DBUILD_LIBTYPE_SHARED
INCLUDE_PATHS = -I. -Iexternal/glfw/include -Iexternal/glfw/deps/mingw
# Raspberry Pi specific include paths:
#INCLUDE_PATHS += -I$(TARG_ROOTFS)/opt/vc/include
#INCLUDE_PATHS += -I$(TARG_ROOTFS)/opt/vc/include/interface/vmcs_host/linux
#INCLUDE_PATHS += -I$(TARG_ROOTFS)/opt/vc/include/interface/vcos/pthreads

# === Raylib Config ===
RAYLIB_VERSION = 2.5.0
RAYLIB_API_VERSION = 2
GRAPHICS = GRAPHICS_API_OPENGL_ES2
PLATFORM = PLATFORM_RPI
OBJS = core.o \
  shapes.o \
  textures.o \
  text.o \
  utils.o \
  models.o \
  raudio.o \
  raygui.o \
  physac.o

# === Main Make Command ===
raylib: $(OBJS)
  mkdir -p $(OUT_PATH)
  $(CC) -shared -o $(OUT_PATH)/libraylib.so.$(RAYLIB_VERSION) $(OBJS) \
    -Wl,-soname,libraylib.so.$(RAYLIB_API_VERSION) -L$(TARG_ROOTFS)/usr/lib \
    -lbrcmGLESv2 -lbrcmEGL -lpthread -lrt -lm -lbcm_host -ldl
  cd $(OUT_PATH) && ln -fsv libraylib.so.$(RAYLIB_VERSION) \
  libraylib.so.$(RAYLIB_API_VERSION)
  cd $(OUT_PATH) && ln -fsv libraylib.so.$(RAYLIB_API_VERSION) libraylib.so
  # Copying raylib development files to $(RAYLIB_H_INSTALL_PATH).
  cp --update raylib.h $(H_INSTALL_PATH)/raylib.h
  cp --update raymath.h $(H_INSTALL_PATH)/raymath.h
  cp --update rlgl.h $(H_INSTALL_PATH)/rlgl.h
  cp --update physac.h $(H_INSTALL_PATH)/physac.h
  cp --update raygui.h $(H_INSTALL_PATH)/raygui.h
  cp --update ricons.h $(H_INSTALL_PATH)/ricons.h
  cp --update easings.h $(H_INSTALL_PATH)/easings.h

# === Objects ===
core.o : core.c raylib.h rlgl.h utils.h raymath.h camera.h gestures.h
  $(CC) -c $< $(CFLAGS) $(INCLUDE_PATHS) -D$(PLATFORM) -D$(GRAPHICS)

# Compile shapes module
shapes.o : shapes.c raylib.h rlgl.h
  $(CC) -c $< $(CFLAGS) $(INCLUDE_PATHS) -D$(PLATFORM) -D$(GRAPHICS)

# Compile textures module
textures.o : textures.c raylib.h rlgl.h utils.h
  $(CC) -c $< $(CFLAGS) $(INCLUDE_PATHS) -D$(PLATFORM) -D$(GRAPHICS)

# Compile text module
text.o : text.c raylib.h utils.h
  $(CC) -c $< $(CFLAGS) $(INCLUDE_PATHS) -D$(PLATFORM) -D$(GRAPHICS)

# Compile utils module
utils.o : utils.c utils.h
  $(CC) -c $< $(CFLAGS) $(INCLUDE_PATHS) -D$(PLATFORM)

# Compile models module
models.o : models.c raylib.h rlgl.h raymath.h
  $(CC) -c $< $(CFLAGS) $(INCLUDE_PATHS) -D$(PLATFORM) -D$(GRAPHICS)

# Compile audio module
raudio.o : raudio.c raylib.h
  $(CC) -c $< $(CFLAGS) $(INCLUDE_PATHS) -D$(PLATFORM)
    
# Compile raygui module
raygui.o : raygui.c raylib.h
  $(CC) -c $< $(CFLAGS) $(INCLUDE_PATHS) -D$(PLATFORM)
    
# Compile physac module
physac.o : physac.c raylib.h
  $(CC) -c $< $(CFLAGS) $(INCLUDE_PATHS) -D$(PLATFORM) -std=gnu99

# === CLEAN ===
# Clean library, .o files, and development headers
clean:
  rm -fv *.o $(OUT_PATH)/libraylib.so* $(H_INSTALL_PATH)/*.h
