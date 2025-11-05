# Postgres
postgres/start:
        sudo -u postgres pg_ctl -D /var/lib/pgsql/data status || sudo -u postgres pg_ctl -D /var/lib/pgsql/data start

postgres/stop: foreman/rails/stop foreman/frontend/stop
        sudo -u postgres pg_ctl -D /var/lib/pgsql/data status || sudo -u postgres pg_ctl -D /var/lib/pgsql/data stop

# Foreman
## Rails
foreman/rails/start: postgres/start foreman/tmp/pids/server.pid

foreman/tmp/pids/server.pid: postgres/start
        cd foreman && bundle exec rails s -b 0.0.0.0 -p 3000 &

foreman/rails/stop:
        if test -f foreman/tmp/pids/server.pid; then kill $$(cat foreman/tmp/pids/server.pid); tail --pid=$$(cat foreman/tmp/pids/server.pid) -f /dev/null; fi

foreman/rails/restart: foreman/rails/stop foreman/rails/start

## Frontend

foreman/frontend/start: postgres/start
        pgrep npm || ( cd foreman && bundle exec foreman start webpack & )

foreman/frontend/stop:
        if pgrep npm > /dev/null; then process=$$(pgrep npm); kill $${process}; tail --pid=$${process} -f /dev/null; fi

foreman/frontend/restart: foreman/frontend/stop foreman/frontend/start

# General

start: postgres/start foreman/rails/start foreman/frontend/start

stop: foreman/frontend/stop foreman/rails/stop postgres/stop

restart: stop start