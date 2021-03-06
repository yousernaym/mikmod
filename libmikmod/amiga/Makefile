# Makefile for targeting AmigaOS variants using GCC.
# You must specify a target OS on the make command line:
# make amigaos  : build for m68k-amigaos
# make amigaos4 : build for ppc-amigaos4
# make aros     : build for i386-aros
# make morphos  : build for ppc-morphos

ifeq ($(CROSS),)
CC=gcc
AS=as
AR=ar
RANLIB=ranlib
else
CC=$(CROSS)-gcc
AS=$(CROSS)-as
AR=$(CROSS)-ar
RANLIB=$(CROSS)-ranlib
endif

# compiler flags
CFLAGS   =-O2 -Wall
CFLAGS  +=-ffast-math -fno-common
#CFLAGS  +=-g
CPPFLAGS =-DHAVE_LIMITS_H -DHAVE_SNPRINTF -DHAVE_UNISTD_H
INCLUDE  =-I../amiga -I../include
ARFLAGS  = cr
LDFLAGS  =
LDLIBS   =-L. -lmikmod
CPPFLAGS_LIB =-DMIKMOD_BUILD

ifeq ($(AROS),1)
include gcc_aros.mk
endif
ifeq ($(MORPHOS),1)
include gcc_morphos.mk
endif
ifeq ($(AOS3),1)
include gcc_aos3.mk
endif
ifeq ($(AOS4),1)
include gcc_aos4.mk
endif

# disable support for module depackers
#CPPFLAGS_LIB+=-DNO_DEPACKERS
# disable the high quality mixer (build only with the standart mixer)
#CPPFLAGS_LIB+=-DNO_HQMIXER

# support for AHI output:
CPPFLAGS_LIB+=-DDRV_AHI
# support for wav file output:
CPPFLAGS_LIB+=-DDRV_WAV
# support for aiff file output:
CPPFLAGS_LIB+=-DDRV_AIFF
# support for output raw data to a file:
CPPFLAGS_LIB+=-DDRV_RAW

COMPILE_LIB=$(CC) -c $(CFLAGS) $(INCLUDE) $(CPPFLAGS) $(CPPFLAGS_LIB)
COMPILE_APP=$(CC) -c $(CFLAGS) $(INCLUDE) $(CPPFLAGS)

.SUFFIXES:
.SUFFIXES: .o .c

LIB=libmikmod.a
APP=splay splayFP splayMEM splayMEM2 soundeffects

OBJ=objs/drv_ahi.o \
    objs/drv_raw.o \
    objs/drv_aiff.o \
    objs/drv_wav.o \
    objs/drv_nos.o \
    objs/strcasecmp.o \
    objs/load_669.o \
    objs/load_amf.o \
    objs/load_asy.o \
    objs/load_dsm.o \
    objs/load_far.o \
    objs/load_gdm.o \
    objs/load_gt2.o \
    objs/load_it.o \
    objs/load_imf.o \
    objs/load_m15.o \
    objs/load_med.o \
    objs/load_mod.o \
    objs/load_mtm.o \
    objs/load_okt.o \
    objs/load_s3m.o \
    objs/load_stm.o \
    objs/load_stx.o \
    objs/load_ult.o \
    objs/load_umx.o \
    objs/load_uni.o \
    objs/load_xm.o \
    objs/mmalloc.o \
    objs/mmerror.o \
    objs/mmio.o \
    objs/mdriver.o \
    objs/mdreg.o \
    objs/mmcmp.o \
    objs/pp20.o \
    objs/s404.o \
    objs/xpk.o \
    objs/mloader.o \
    objs/mlreg.o \
    objs/mlutil.o \
    objs/mplayer.o \
    objs/munitrk.o \
    objs/mwav.o \
    objs/npertab.o \
    objs/sloader.o \
    objs/virtch.o \
    objs/virtch2.o \
    objs/virtch_common.o
#   objs/mdulaw.o

all: $(LIB) $(APP)

aros:
	@echo Building for i386-aros
	$(MAKE) AROS=1
morphos:
	@echo Building for ppc-morphos
	$(MAKE) MORPHOS=1
amigaos:
	@echo Building for m68k-amigaos
	$(MAKE) AOS3=1
amigaos4:
	@echo Building for ppc-amigaos4
	$(MAKE) AOS4=1

clean:
	rm -f $(APP) $(LIB) *.lib objs/*.o

$(LIB): $(OBJ)
	$(AR) $(ARFLAGS) $@ $(OBJ)
	$(RANLIB) $@

splay: $(LIB) objs/amiga.o objs/splay.o
	$(CC) objs/amiga.o objs/splay.o $(LDFLAGS) $(LDLIBS) -o splay
splayFP: $(LIB) objs/amiga.o objs/splayFP.o
	$(CC) objs/amiga.o objs/splayFP.o $(LDFLAGS) $(LDLIBS) -o splayFP
splayMEM: $(LIB) objs/amiga.o objs/myloader.o objs/splayMEM.o
	$(CC) objs/amiga.o objs/myloader.o objs/splayMEM.o $(LDFLAGS) $(LDLIBS) -o splayMEM
splayMEM2: $(LIB) objs/amiga.o objs/splayMEM2.o
	$(CC) objs/amiga.o objs/splayMEM2.o $(LDFLAGS) $(LDLIBS) -o splayMEM2
soundeffects: $(LIB) objs/amiga.o objs/soundeffects.o
	$(CC) objs/amiga.o objs/soundeffects.o $(LDFLAGS) $(LDLIBS) -o soundeffects

HEADER_DEPS=../include/mikmod.h ../include/mikmod_internals.h ../include/mikmod_ctype.h
objs/drv_ahi.o: ../drivers/drv_ahi.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../drivers/drv_ahi.c -o objs/drv_ahi.o
objs/drv_nos.o: ../drivers/drv_nos.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../drivers/drv_nos.c -o objs/drv_nos.o
objs/drv_raw.o: ../drivers/drv_raw.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../drivers/drv_raw.c -o objs/drv_raw.o
objs/drv_aiff.o: ../drivers/drv_aiff.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../drivers/drv_aiff.c -o objs/drv_aiff.o
objs/drv_wav.o: ../drivers/drv_wav.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../drivers/drv_wav.c -o objs/drv_wav.o
objs/load_669.o: ../loaders/load_669.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_669.c -o objs/load_669.o
objs/load_amf.o: ../loaders/load_amf.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_amf.c -o objs/load_amf.o
objs/load_asy.o: ../loaders/load_asy.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_asy.c -o objs/load_asy.o
objs/load_dsm.o: ../loaders/load_dsm.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_dsm.c -o objs/load_dsm.o
objs/load_far.o: ../loaders/load_far.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_far.c -o objs/load_far.o
objs/load_gdm.o: ../loaders/load_gdm.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_gdm.c -o objs/load_gdm.o
objs/load_gt2.o: ../loaders/load_gt2.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_gt2.c -o objs/load_gt2.o
objs/load_it.o: ../loaders/load_it.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_it.c -o objs/load_it.o
objs/load_imf.o: ../loaders/load_imf.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_imf.c -o objs/load_imf.o
objs/load_m15.o: ../loaders/load_m15.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_m15.c -o objs/load_m15.o
objs/load_med.o: ../loaders/load_med.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_med.c -o objs/load_med.o
objs/load_mod.o: ../loaders/load_mod.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_mod.c -o objs/load_mod.o
objs/load_mtm.o: ../loaders/load_mtm.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_mtm.c -o objs/load_mtm.o
objs/load_okt.o: ../loaders/load_okt.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_okt.c -o objs/load_okt.o
objs/load_s3m.o: ../loaders/load_s3m.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_s3m.c -o objs/load_s3m.o
objs/load_stm.o: ../loaders/load_stm.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_stm.c -o objs/load_stm.o
objs/load_stx.o: ../loaders/load_stx.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_stx.c -o objs/load_stx.o
objs/load_ult.o: ../loaders/load_ult.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_ult.c -o objs/load_ult.o
objs/load_umx.o: ../loaders/load_umx.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_umx.c -o objs/load_umx.o
objs/load_uni.o: ../loaders/load_uni.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_uni.c -o objs/load_uni.o
objs/load_xm.o: ../loaders/load_xm.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../loaders/load_xm.c -o objs/load_xm.o
objs/mmalloc.o: ../mmio/mmalloc.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../mmio/mmalloc.c -o objs/mmalloc.o
objs/mmerror.o: ../mmio/mmerror.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../mmio/mmerror.c -o objs/mmerror.o
objs/mmio.o: ../mmio/mmio.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../mmio/mmio.c -o objs/mmio.o
objs/mmcmp.o: ../depackers/mmcmp.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../depackers/mmcmp.c -o objs/mmcmp.o
objs/pp20.o: ../depackers/pp20.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../depackers/pp20.c -o objs/pp20.o
objs/s404.o: ../depackers/s404.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../depackers/s404.c -o objs/s404.o
objs/xpk.o: ../depackers/xpk.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../depackers/xpk.c -o objs/xpk.o
objs/mdriver.o: ../playercode/mdriver.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../playercode/mdriver.c -o objs/mdriver.o
objs/mdreg.o: ../playercode/mdreg.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../playercode/mdreg.c -o objs/mdreg.o
objs/mdulaw.o: ../playercode/mdulaw.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../playercode/mdulaw.c -o objs/mdulaw.o
objs/mloader.o: ../playercode/mloader.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../playercode/mloader.c -o objs/mloader.o
objs/mlreg.o: ../playercode/mlreg.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../playercode/mlreg.c -o objs/mlreg.o
objs/mlutil.o: ../playercode/mlutil.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../playercode/mlutil.c -o objs/mlutil.o
objs/mplayer.o: ../playercode/mplayer.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../playercode/mplayer.c -o objs/mplayer.o
objs/munitrk.o: ../playercode/munitrk.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../playercode/munitrk.c -o objs/munitrk.o
objs/mwav.o: ../playercode/mwav.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../playercode/mwav.c -o objs/mwav.o
objs/npertab.o: ../playercode/npertab.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../playercode/npertab.c -o objs/npertab.o
objs/sloader.o: ../playercode/sloader.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../playercode/sloader.c -o objs/sloader.o
objs/virtch.o: ../playercode/virtch.c ../playercode/virtch_common.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../playercode/virtch.c -o objs/virtch.o
objs/virtch2.o: ../playercode/virtch2.c ../playercode/virtch_common.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../playercode/virtch2.c -o objs/virtch2.o
objs/virtch_common.o: ../playercode/virtch_common.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../playercode/virtch_common.c -o objs/virtch_common.o
objs/strcasecmp.o: ../posix/strcasecmp.c $(HEADER_DEPS)
	$(COMPILE_LIB) ../posix/strcasecmp.c -o objs/strcasecmp.o

objs/amiga.o:  ../examples/amiga.c
	$(COMPILE_APP) ../examples/amiga.c -o objs/amiga.o
objs/splay.o: ../examples/simpleplay/splay.c $(HEADER_DEPS)
	$(COMPILE_APP) ../examples/simpleplay/splay.c -o objs/splay.o
objs/splayFP.o: ../examples/simpleplayFP/splayFP.c $(HEADER_DEPS)
	$(COMPILE_APP) ../examples/simpleplayFP/splayFP.c -o objs/splayFP.o
objs/myloader.o: ../examples/simpleplayMEM/myloader.c ../examples/simpleplayMEM/myloader.h $(HEADER_DEPS)
	$(COMPILE_APP) -I../examples/simpleplayMEM ../examples/simpleplayMEM/myloader.c -o objs/myloader.o
objs/splayMEM.o: ../examples/simpleplayMEM/splayMEM.c ../examples/simpleplayMEM/myloader.h $(HEADER_DEPS)
	$(COMPILE_APP) -I../examples/simpleplayMEM ../examples/simpleplayMEM/splayMEM.c -o objs/splayMEM.o
objs/splayMEM2.o: ../examples/simpleplayMEM2/splayMEM2.c $(HEADER_DEPS)
	$(COMPILE_APP) ../examples/simpleplayMEM2/splayMEM2.c -o objs/splayMEM2.o
objs/soundeffects.o: ../examples/soundeffects/soundeffects.c $(HEADER_DEPS)
	$(COMPILE_APP) ../examples/soundeffects/soundeffects.c -o objs/soundeffects.o

