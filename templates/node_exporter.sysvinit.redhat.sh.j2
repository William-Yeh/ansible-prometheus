#!/bin/bash

### BEGIN INIT INFO
# processname:       node_exporter
# Short-Description: Exporter for machine metrics.
# Description:       Prometheus exporter for machine metrics,
#                    written in Go with pluggable metric collectors.
#
# chkconfig: 2345 80 80
# pidfile: {{ prometheus_pid_path }}/node_exporter.pid
#
#
### END INIT INFO

#set -e

# Source function library.
. /etc/init.d/functions


NAME=node_exporter
DESC="Exporter for machine metrics"
DAEMON={{ prometheus_node_exporter_daemon_dir }}/node_exporter
USER={{ prometheus_user }}
CONFIG=
PID="{{ prometheus_pid_path }}/$NAME.pid"
LOG="{{ prometheus_log_path }}/$NAME.log"
GOSU=/usr/local/bin/gosu

{% if prometheus_node_exporter_opts is defined %}
DAEMON_OPTS='{{ prometheus_node_exporter_opts }}'
{% else %}
DAEMON_OPTS=
{% endif %}

RETVAL=0



# Check if DAEMON binary exist
[ -f $DAEMON ] || exit 0

[ -f "/etc/default/$NAME" ]  &&  . /etc/default/$NAME


service_not_configured () {
  if [ "$1" != "stop" ]; then
    printf "\tPlease configure $NAME and then edit /etc/default/$NAME\n"
    printf "\tand set the \"START\" variable to \"yes\" in order to allow\n"
    printf "\t$NAME to start.\n"
  fi
  exit 0
}

service_checks() {
  # Check if START variable is set to "yes", if not we exit.
  if [ "$START" != "yes" ]; then
    service_not_configured $1
  fi

  # Prepare directories
  mkdir -p "{{ prometheus_pid_path }}" "{{ prometheus_log_path }}"
  chown -R $USER "{{ prometheus_pid_path }}" "{{ prometheus_log_path }}"

  # Check if PID exists
  if [ -f "$PID" ]; then
    PID_NUMBER=`cat $PID`
    if [ -z "`ps axf | grep ${PID_NUMBER} | grep -v grep`" ]; then
      echo "Service was aborted abnormally; clean the PID file and continue..."
      rm -f "$PID"
    else
      echo "Service already started; skip..."
      exit 1
    fi
  fi
}

start() {
  service_checks $1
  $GOSU $USER   $DAEMON $DAEMON_OPTS  > $LOG 2>&1  &
  RETVAL=$?
  echo $! > $PID
}

stop() {
  killproc -p $PID -b $DAEMON  $NAME
  RETVAL=$?
}

reload() {
  #-- sorry but node_exporter doesn't handle -HUP signal...
  #killproc -p $PID -b $DAEMON  $NAME -HUP
  #RETVAL=$?
  stop
  start
}

case "$1" in
  start)
    echo -n $"Starting $DESC -" "$NAME" $'\n'
    start
    ;;

  stop)
    echo -n $"Stopping $DESC -" "$NAME" $'\n'
    stop
    ;;

  reload)
    echo -n $"Reloading $DESC configuration -" "$NAME" $'\n'
    reload
    ;;

  restart|force-reload)
    echo -n $"Restarting $DESC -" "$NAME" $'\n'
    stop
    start
    ;;

  syntax)
    $DAEMON --help
    ;;

  status)
    status -p $PID $DAEMON
    ;;

  *)
    echo -n $"Usage: /etc/init.d/$NAME {start|stop|reload|restart|force-reload|syntax|status}" $'\n'
    ;;
esac

exit $RETVAL
