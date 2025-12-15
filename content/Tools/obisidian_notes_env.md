## main goal
the main goal of this project is to create a seamless, self-hosted and cheap alternative to the note-taking apps.

[tutorial](https://blog.kirillov.cc/posts/obsidian-livesync/)

## current architecture:
- VM (google cloud storage) with CouchDB


## future architecture expansions:
- fixed domain, caddy reverse proxy with HTTPS
	- Obsidian Mobile only works with HTTPS
	- GCP VMs IP changes frequently
	- caddy has a custom image with cloudflare DNS for automatic management of SSL certificates [caddy-cloudflare](https://github.com/CaddyBuilds/caddy-cloudflare)
- integrate reminders and bookmarks
- attach a static site generator to pull the notes from the DB and generate and deploy the static site



## troubleshooting
- CouchDB on the second startup after the first configuration fails to start because of missing `_users` database. [Fix](https://stackoverflow.com/questions/69542519/why-is-couchdb-looking-for-users-database)
```
curl -X PUT http://<COUCHDB_USER>:<COUCHDB_PASSWORD>@127.0.0.1:5984/_users

curl -X PUT http://<COUCHDB_USER>:<COUCHDB_PASSWORD>@127.0.0.1:5984/_replicator

curl -X PUT http://<COUCHDB_USER>:<COUCHDB_PASSWORD>@127.0.0.1:5984/_global_changes
```

- obsidian sync can have CORS issues due to CouchDB config
	- to fix open the web interface `http://127.0.0.1:5984/_utils` and activate CORS under Configuration > CORS (for testing purpose allow all origins, for production it's always better to give access to predefined origins)
	- [github issue](https://github.com/vrtmrz/obsidian-livesync/issues/352)