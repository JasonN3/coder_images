postgres/start:
	sudo -u postgres pg_ctl -D /var/lib/pgsql/data status || sudo -u postgres pg_ctl -D /var/lib/pgsql/data start

foreman/rails/start: postgres/start
	bundle exec rails s -b 0.0.0.0 -p 3000

foreman/frontend/start: postgres/start
	bundle exec foreman start webpack

start: postgres/start foreman/rails foreman/frontend

postgres/stop:
	sudo -u postgres pg_ctl -D /var/lib/pgsql/data status || sudo -u postgres pg_ctl -D /var/lib/pgsql/data stop

stop: postgres/stop foreman/rails/stop foreman/frontend/stop
