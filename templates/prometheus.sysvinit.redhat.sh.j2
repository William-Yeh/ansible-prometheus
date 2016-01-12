#!/bin/bash

### BEGIN INIT INFO
# processname:       prometheus
# Short-Description: monitoring system and time series database.
# Description:       Prometheus is a systems and service monitoring system.
#                    It collects metrics from configured targets at given intervals,
#                    evaluates rule expressions, displays the results,
#                    and can trigger alerts if some condition is observed to be true.
#
# chkconfig: 2345 80 80
# config:  {{ prometheus_config_path }}/prometheus.yml
# pidfile: {{ prometheus_pid_path }}/prometheus.pid
#
#
### END INIT INFO

#set -e

# Source function library.
. /etc/init.d/functions


NAME=prometheus
DESC="Prometheus monitoring system"
DAEMON={{ prometheus_daemon_dir }}/prometheus
USER={{ prometheus_user }}
CONFIG={{ prometheus_config_path }}/prometheus.yml
PID="{{ prometheus_pid_path }}/$NAME.pid"
LOG="{{ prometheus_log_path }}/$NAME.log"
GOSU=/usr/local/bin/gosu

RETVAL=0


{% if prometheus_alertmanager_url is defined %}
ALERTMANAGER_OPTS="-alertmanager.url={{ prometheus_alertmanager_url }}"
{% else %}
ALERTMANAGER_OPTS=
{% endif %}



DAEMON_OPTS="$ALERTMANAGER_OPTS"
DAEMON_OPTS="$DAEMON_OPTS -config.file=$CONFIG  -storage.local.path={{ prometheus_db_path }}"
DAEMON_OPTS="$DAEMON_OPTS -web.console.templates={{ prometheus_daemon_dir }}/consoles  -web.console.libraries={{ prometheus_daemon_dir }}/console_libraries"
{% if prometheus_opts is defined %}
DAEMON_OPTS="$DAEMON_OPTS {{ prometheus_opts }}"
{% endif %}


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
  killproc -p $PID -b $DAEMON  $NAME -HUP
  RETVAL=$?
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
