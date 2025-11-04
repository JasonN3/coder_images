postgres/start:
	sudo -u postgres pg_ctl -D /var/lib/pgsql/data status || sudo -u postgres pg_ctl -D /var/lib/pgsql/data start

foreman/rails/start: postgres/start foreman/tmp/pids/server.pid

foreman/tmp/pids/server.pid: postgres/start
	cd foreman && bundle exec rails s -b 0.0.0.0 -p 3000 &

foreman/frontend/start: postgres/start
	cd foreman && bundle exec foreman start webpack &

start: postgres/start foreman/rails/start foreman/frontend/start

postgres/stop:
	sudo -u postgres pg_ctl -D /var/lib/pgsql/data status || sudo -u postgres pg_ctl -D /var/lib/pgsql/data stop

foreman/rails/stop:
	kill $(cat foreman/tmp/pids/server.pid)

stop: postgres/stop foreman/rails/stop foreman/frontend/stop
