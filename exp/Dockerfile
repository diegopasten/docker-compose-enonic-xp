FROM enonic/xp-app:6.13.1

ENV SNAPSHOTTER_VERSION 1.0.5
# Set Memory settings
# Defaults to 1/4 of servers physical mem
# On single cpu
# ENV JAVA_OPTS "-Xms2G -Xmx2G"
# On multi cpu systems
# ENV JAVA_OPTS "-Xms5G -Xmx5G -Djsse.enableSNIExtension=true -XX:+UseConcMarkSweepGC -XX:+CMSParallelRemarkEnabled -XX:+UseCMSInitiatingOccupancyOnly -XX:CMSInitiatingOccupancyFraction=60 -XX:+ScavengeBeforeFullGC -XX:+CMSScavengeBeforeRemark"
# Dump memory to heapdum file on outofmem
ENV JAVA_OPTS="$JAVA_OPTS -XX:-HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/heapdump.hprof"

USER root

RUN cp -r $XP_ROOT/home.org $XP_ROOT/home

###############################################################################
#    Insert commands to place Enonic XP application in deploy folder here.    #
###############################################################################

# Either place the compiled jar file in the deploy folder...
COPY deploy/* $XP_ROOT/home/deploy/

# Or download it from an alternative source on the net.
#ENV SUPERHERO_VERSION 1.0.0
#RUN wget -O $XP_ROOT/home/deploy/superhero-$SUPERHERO_VERSION.jar http://repo.enonic.com/public/com/enonic/app/superhero/$SUPERHERO_VERSION/superhero-$SUPERHERO_VERSION.jar
RUN wget -O $XP_ROOT/home/deploy/snapshotter-$SNAPSHOTTER_VERSION.jar http://repo.enonic.com/public/com/enonic/app/snapshotter/$SNAPSHOTTER_VERSION/snapshotter-$SNAPSHOTTER_VERSION.jar
RUN mkdir -p /enonic-xp/home/snapshots && chown $XP_USER /enonic-xp/home/snapshots
###############################################################################

COPY config/* $XP_ROOT/home/config/

COPY backup.sh /usr/local/bin/backup.sh
RUN chmod +x /usr/local/bin/backup.sh

RUN mkdir $XP_ROOT/home/repo
RUN chown enonic-xp -R $XP_ROOT/home

USER enonic-xp
