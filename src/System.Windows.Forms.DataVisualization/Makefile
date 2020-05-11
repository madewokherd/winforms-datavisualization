
RESX2SR=mono $(WINE_MONO_SRCDIR)/mono/mcs/class/lib/build-linux/resx2sr.exe

COMPILE_LIB=csc -target:library

LIB_FLAGS=-unsafe -delaysign+ -keyfile:../../winfx3.pub -define:WINFORMS_CONTROL -langversion:7.3 -lib:$(MONO_PREFIX)/lib/mono/4.5-api

LIB_REFS=-r:mscorlib.dll -r:System.dll -r:System.Configuration.dll -r:System.Core.dll -r:System.Drawing.dll -r:System.Xml.dll

LIB_SRCS=$(shell cat sources.list)

all: System.Windows.Forms.DataVisualization.dll

SR.resources.cs: Common/SR.resx
	$(RESX2SR) $< > $@ || rm -f $@

%.resources: %.resx
	resgen $< $@

EMBEDDED_RESOURCES=$(shell cat embeddedresources.list)

EMBEDDED_RES_PATHS=$(EMBEDDED_RESOURCES)

embeddedresources.flags: embeddedresources.list
	for i in `cat embeddedresources.list`; do printf -- '-resource:%s,%s\n' $$i `echo $$i|sed "s:/:.:g"|sed "s:Common.SR:System.Windows.Forms.DataVisualization.Charting.SR:g"`; done > $@ || rm -f $@

System.Windows.Forms.DataVisualization.dll: sources.list $(LIB_SRCS) $(SIBLING_PATHS) embeddedresources.flags $(EMBEDDED_RES_PATHS)
	$(COMPILE_LIB) $(LIB_FLAGS) $(LIB_REFS) $(SIBLING_REFS) -out:$@ @sources.list @embeddedresources.flags
	sn -R $@ ../../mono.snk
