#!/bin/bash

### BEGIN INIT INFO
# processname:       alertmanager
# Short-Description: Prometheus Alertmanager.
# Description:       The Alertmanager receives alerts from one or more Prometheus servers.
#                    It manages those alerts, including silencing, inhibition, aggregation
#                    and sending out notifications via methods such as email, PagerDuty and HipChat.
#
# chkconfig: 2345 80 80
# config:  {{ prometheus_config_path }}/alertmanager.yml
# pidfile: {{ prometheus_pid_path }}/alertmanager.pid
#
#
### END INIT INFO

#set -e

# Source function library.
. /etc/init.d/functions


NAME=alertmanager
DESC="Prometheus Alertmanager"
DAEMON={{ prometheus_alertmanager_daemon_dir }}/alertmanager
USER={{ prometheus_user }}
CONFIG={{ prometheus_config_path }}/alertmanager.yml
PID="{{ prometheus_pid_path }}/$NAME.pid"
LOG="{{ prometheus_log_path }}/$NAME.log"
CHDIR="{{ prometheus_alertmanager_db_path }}"
GOSU=/usr/local/bin/gosu



DAEMON_OPTS="-config.file=$CONFIG  -storage.path={{ prometheus_alertmanager_db_path }} "

{% if prometheus_alertmanager_opts is defined %}
DAEMON_OPTS="$DAEMON_OPTS {{ prometheus_alertmanager_opts }}"
{% endif %}

RETVAL=0




# Check if DAEMON binary exist
[ -f $DAEMON ] || exit 0

[ -f "/etc/default/$NAME" ]  &&  . /etc/default/$NAME


service_not_configured() {
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
  mkdir -p "{{ prometheus_pid_path }}" "{{ prometheus_log_path }}" "{{ prometheus_alertmanager_db_path }}"
  chown -R $USER "{{ prometheus_pid_path }}" "{{ prometheus_log_path }}" "{{ prometheus_alertmanager_db_path }}"

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

#chdir $CHDIR

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
