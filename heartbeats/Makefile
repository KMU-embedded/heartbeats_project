#CXX = /usr/bin/gcc
CXX = arm-linux-gnueabihf-gcc
CXXFLAGS = -Wall -Wno-unknown-pragmas -Iinc -Llib -O6
DBG = -g
DEFINES ?= 
#LDFLAGS = -lpthread -lrt -lhb-file -lhrm-file
LDFLAGS = -lpthread -lrt -lhb-shared -lhrm-shared

DOCDIR = doc
BINDIR = bin
LIBDIR = lib
INCDIR = ./inc
SCRATCH = ./scratch
OUTPUT = ./output
SRCDIR = ./src
ROOTS = heart_rate_monitor_test
TEST_ROOTS = test1 test2
BINS = $(ROOTS:%=$(BINDIR)/%)
TESTS = $(TEST_ROOTS:%=$(BINDIR)/%)
OBJS = $(ROOTS:%=$(BINDIR)/%.o)
TEST_OBJS = $(TEST_ROOTS:%=$(BINDIR)/%.o)

all: hb-shared

$(BINDIR):
	-mkdir -p $(BINDIR)

$(LIBDIR):
	-mkdir -p $(LIBDIR)

$(SCRATCH):
	-mkdir -p $(SCRATCH)

$(OUTPUT):
	-mkdir -p $(OUTPUT)


$(BINDIR)/%.o : $(SRCDIR)/%.c
	$(CXX) -c $(CXXFLAGS) $(DEFINES) $(DBG) -o $@ $<


$(BINS) : $(OBJS) 

$(BINS) : % : %.o
	$(CXX) $(CXXFLAGS) -o $@ $< $(LDFLAGS)

$(TESTS) : $(TEST_OBJS) 

$(TESTS) : % : %.o
	$(CXX) $(CXXFLAGS) -o $@ $< $(LDFLAGS)



# Heartbeat shared memory version
hb-shared: $(BINDIR) $(LIBDIR) $(SCRATCH) hblib-shared $(OUTPUT) $(BINS)

hblib-shared: $(LIBDIR)/libhb-shared.a $(LIBDIR)/libhrm-shared.a

$(LIBDIR)/libhb-shared.a: $(SRCDIR)/heartbeat-shared.c $(INCDIR)/heartbeat.h
	$(MAKE) $(BINDIR)/heartbeat-shared.o
	ar r $(LIBDIR)/libhb-shared.a $(BINDIR)/heartbeat-shared.o
	ranlib $(LIBDIR)/libhb-shared.a

$(LIBDIR)/libhrm-shared.a: $(SRCDIR)/heart_rate_monitor-shared.c $(INCDIR)/heart_rate_monitor.h
	$(MAKE) $(BINDIR)/heart_rate_monitor-shared.o
	ar r $(LIBDIR)/libhrm-shared.a $(BINDIR)/heart_rate_monitor-shared.o
	ranlib $(LIBDIR)/libhrm-shared.a




## cleaning
clean:
	-rm -rf $(BINDIR) $(LIBDIR) $(SCRATCH) *.log *~ $(SRCDIR)/*~

squeaky: clean
	-rm -rf $(OUTPUT)

# Documentation
documentation: 
	doxygen heartbeats_doc

clean-documentation:
	-rm -rf $(DOCDIR)/html/*
	-rm -rf $(DOCDIR)/latex/*
