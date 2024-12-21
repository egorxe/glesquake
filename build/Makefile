#########################################################################
# Build Quake
#########################################################################

BUILD_PATH := .
SRC_PATH := ../src

#DEFINES = -DNO_BLENDING=1 -DNO_MULTITEXTURE=1
DEFINES = -DNO_MULTITEXTURE=1
CXXFLAGS += -g -DUSE_GLES=1 -std=c++03 -Wno-write-strings $(DEFINES)

ifeq ($(RISCV),1)
export RISCV = 1
export CC = riscv64-linux-gnu-gcc
export CXX = riscv64-linux-gnu-g++
CXXFLAGS += -O2 -DNDEBUG -DUSE_GLES=1 -std=c++03 -Wno-write-strings $(DEFINES) -g
endif

PSEUDOGL_PATH ?= $(HOME)/proj/gpu/openglory/sw/pseudogl
PSEUDOGL_LIB = $(PSEUDOGL_PATH)/libpseudogl.a

SRCS := \
  cd_null.cpp \
  cl_demo.cpp \
  cl_input.cpp \
  cl_main.cpp \
  cl_parse.cpp \
  cl_tent.cpp \
  chase.cpp \
  cmd.cpp \
  common.cpp \
  console.cpp \
  crc.cpp \
  cvar.cpp \
  gl_draw.cpp \
  gl_mesh.cpp \
  gl_model.cpp \
  gl_refrag.cpp \
  gl_rlight.cpp \
  gl_rmain.cpp \
  gl_rmisc.cpp \
  gl_rsurf.cpp \
  gl_screen.cpp \
  gl_vidandroid.cpp \
  gl_warp.cpp \
  host.cpp \
  host_cmd.cpp \
  keys.cpp \
  mathlib.cpp \
  menu.cpp \
  net_bsd.cpp \
  net_dgrm.cpp \
  net_loop.cpp \
  net_main.cpp \
  net_vcr.cpp \
  net_udp.cpp \
  nonintel.cpp \
  pr_cmds.cpp \
  pr_edict.cpp \
  pr_exec.cpp \
  r_part.cpp \
  sbar.cpp \
  snd_null.cpp \
  sv_main.cpp \
  sv_phys.cpp \
  sv_move.cpp \
  sv_user.cpp \
  sys_linux.cpp \
  view.cpp \
  wad.cpp \
  world.cpp \
  zone.cpp

OBJS=$(SRCS:%.cpp=$(BUILD_PATH)/%.o )

OGLORY_BIN = $(BUILD_PATH)/quake.oglory
SDL_BIN = $(BUILD_PATH)/quake.sdl

.PHONY: default sdl oglory clean

default: oglory

sdl: CXXFLAGS += -DUSE_SDL=1 
sdl: $(SDL_BIN)

oglory: CXXFLAGS += -I$(PSEUDOGL_PATH)/include
oglory: $(OGLORY_BIN)

clean:
	rm -f $(BUILD_PATH)/*.o $(SDL_BIN) $(OGLORY_BIN)

$(BUILD_PATH)/%.o: $(SRC_PATH)/%.cpp
	$(CXX) $(CXXFLAGS) -c $< -o $@

$(PSEUDOGL_LIB):
	make -C $(PSEUDOGL_PATH) -j8

$(OGLORY_BIN): $(OBJS) $(PSEUDOGL_LIB)
	$(CXX) $(CXXFLAGS) $(OBJS) -L$(PSEUDOGL_PATH) $(PSEUDOGL_LIB) -o $@
	
$(SDL_BIN): $(OBJS)
	$(CXX) $(CXXFLAGS) $(OBJS) -lEGL -lGLESv1_CM -lSDL2 -o $@
