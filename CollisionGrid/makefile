CXX = g++

PACKAGENAME = CollisionGrid

include ../../makefile-common

INCLUDES = -I. -I../../Core/Inc -I../../Engine/Inc -I../../CacusLib -I/usr/include/i386-linux-gnu/

#CXXFLAGS = -std=gnu++17 -m32 -msse -msse2 -O3 -mfpmath=sse -rdynamic -fPIC -flto -fomit-frame-pointer -fpermissive -fno-unwind-tables -fno-asynchronous-unwind-tables -fno-strict-aliasing -ffunction-sections -fvisibility=hidden -Werror

#LDFLAGS = -shared -m32 -export-dynamic -Wl,--gc-sections -Wl,-soname,$(SONAME)

LIBS = ../../System/Core.so ../../System/Engine.so 

SRCS = API_Generic.cpp CollisionGrid.cpp ContainerLogic.cpp GridQueries.cpp

OBJS = $(SRCS:%.cpp=$(OBJDIR)%.o)

DEPS = $(SRCS:%.cpp=$(OBJDIR)%.d)


.PHONY: all
all: $(OUTPUT)

#Link
$(OUTPUT) : $(OBJS)
	$(CXX) -o $@ $(LDFLAGS) $^ $(LIBS)
	strip --strip-all --discard-all $(OUTPUT)

#Compile
$(OBJS) : $(OBJDIR)%.o : %.cpp
	$(CXX) -c $(PREPROCESSORS) $(CXXFLAGS) $(INCLUDES) -o $@ $< > $(OBJDIR)$*.lst

#Generate dependancies
#%.d : %.cpp
$(DEPS) : $(OBJDIR)%.d : %.cpp
	@mkdir -p $(OBJDIR)
	@echo $(@:%.d=%.o) $@ : \\ > $@
	@/bin/sh -ec '$(CXX) -MM $(PREPROCESSORS) $(CXXFLAGS) $(INCLUDES) $< | \
		sed -e '\''s/.*://'\'' >> $@'

include $(DEPS)

.PHONY: clean
clean:
	-${RM} -f ${OBJS} $(SRCS:%.cpp=$(OBJDIR)%.lst) $(SRCS:%.cpp=$(OBJDIR)%.d)

