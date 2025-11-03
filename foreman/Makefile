clone: foreman

foreman:
	git clone https://github.com/theforeman/foreman.git
	cd foreman && cp config/settings.yaml.example config/settings.yaml
	cd foreman && cp config/database.yml.example config/database.yml

install: foreman/install

foreman/install: foreman
	cd foreman && bundle install
	cd foreman && npm install

postgres/start:
	sudo -u postgres pg_ctl -D /var/lib/pgsql/data status || sudo -u postgres pg_ctl -D /var/lib/pgsql/data start

postgres/initdb: postgres/start
	test -f .initdb || bundle exec rake db:create
	test -f .initdb || bundle exec rake db:migrate
	test -f .initdb || SEED_ADMIN_PASSWORD=admin bundle exec rake db:seed
	test -f .initdb || touch .initdb

start: postgres/start foreman/rails foreman/frontend

foreman/rails: postgres/start
	bundle exec rails s -b 0.0.0.0 -p 3000

foreman/frontend: postgres/start
	bundle exec foreman start webpack

postgres/stop:
	sudo -u postgres pg_ctl -D /var/lib/pgsql/data status || sudo -u postgres pg_ctl -D /var/lib/pgsql/data stop
