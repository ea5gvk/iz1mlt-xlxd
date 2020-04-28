CXX=g++
CXXFLAGS=-Os -Wall -std=c++11 -pthread
LDFLAGS=-Os -Wall -std=c++11 -pthread
SOURCES=$(wildcard *.cpp)
OBJECTS=$(SOURCES:.cpp=.o)
EXECUTABLE=xlxd

all: $(SOURCES) $(EXECUTABLE)

$(EXECUTABLE): $(OBJECTS)
	$(CXX) $(LDFLAGS) $(OBJECTS) -o $@

.cpp.o:
	$(CXX) $(CXXFLAGS) -c -o $@ $<

clean:
	$(RM) $(EXECUTABLE) *.o *.d *.bak

install:
	mkdir -p /var/log/xlxd
	mkdir -p /etc/xlxd
	cp -f $(EXECUTABLE) /usr/local/bin
	[ -f /etc/xlxd//xlxd.blacklist ] && \
		cp ../config/xlxd.blacklist /etc/xlxd/xlxd.blacklist.sample || \
		cp ../config/xlxd.blacklist /etc/xlxd/xlxd.blacklist
	[ -f /etc/xlxd/xlxd.whitelist ] && \
		cp ../config/xlxd.whitelist /etc/xlxd/xlxd.whitelist.sample || \
		cp ../config/xlxd.whitelist /etc/xlxd/xlxd.whitelist
	[ -f /etc/xlxd/xlxd.interlink ] && \
		cp ../config/xlxd.interlink /etc/xlxd/xlxd.interlink.sample || \
		cp ../config/xlxd.interlink /etc/xlxd/xlxd.interlink
	[ -f /etc/xlxd/xlxd.terminal ] && \
		cp ../config/xlxd.terminal /etc/xlxd/xlxd.terminal.sample || \
		cp ../config/xlxd.terminal /etc/xlxd/xlxd.terminal
	cp -r ../Systemd/xlxd.service /lib/systemd/system/xlxd.service
	cp -r ../Systemd/xlxd.timer /lib/systemd/system/xlxd.timer
	cp -r ../Systemd/xlxd /etc/init.d/xlxd
	systemctl daemon-reload
	systemctl enable xlxd.timer

update:
	cp -f $(EXECUTABLE) /usr/local/bin

remove:
	systemctl disable xlxd.timer
	rm -r /usr/local/bin/xlxd
	rm -r /etc/init.d/xlxd
	rm -r /etc/xlxd
	rm -r /var/log/xlxd
	rm -r /lib/systemd/system/xlxd.*
	systemctl daemon-reload
