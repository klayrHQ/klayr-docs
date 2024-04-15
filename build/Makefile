.PHONY: all
.PHONY: ui
.PHONY: antora
.PHONY: build
.PHONY: typedoc
.PHONY: git-page
.PHONY: deploy-local
.PHONY: clean

all: ui antora
deploy: clean build git-page
deploy-local: clean build

ui:
	cd antora && git pull origin antora && npm ci && gulp bundle
antora: ui
	antora site.yml --fetch
build: typedoc
	npm ci
	source .env && antora live-site.yml --fetch
	cd typedoc/klayr-sdk && cp -r docs/typedoc ../../../live/klayr-sdk/references/typedoc
typedoc:
	mkdir typedoc
	cd typedoc && git clone https://github.com/klayrhq/klayr-sdk.git && cd klayr-sdk && git checkout feature/1-klayr-rebranding
	cd typedoc/klayr-sdk && npm install -g yarn typedoc && yarn install && ls && yarn build && npx typedoc
git-page:
	rm -rf git-page
	git worktree remove git-page || true
	git worktree add git-page git-page
	cp -r ../live/* git-page
	cd git-page && git add . && git commit -m "Update docs" && git push origin git-page
clean:
	rm -rf node_modules
	rm -rf typedoc
	rm -rf ../live
