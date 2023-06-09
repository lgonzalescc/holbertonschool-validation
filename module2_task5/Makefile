build: hugo-build go-build  ## Build the project

go-build:   ## Compile the source code of the application to a binary
	@go build

hugo-build:  ## Generate the website from the markdown and configuration files in the directory dist/ 
	@hugo

run: build   ## Run the application in background by executing the binary
	@./awesome-api > ./awesome-api.log 2>&1 &

post:   ## Create a new blog post whose filename and title come from the environment variables POST_TITLE and POST_NAME
	@rm -Rf content/posts/$(POST_NAME).md
	@hugo new posts/$(POST_NAME).md
	@sed -i 's/title:.*/title: $(POST_TITLE)/' content/posts/$(POST_NAME).md
	@sed -i 's/draft:.*/draft: "false"/' content/posts/$(POST_NAME).md

stop:   ## Stop the application
	@pkill awesome-api || true

clean: stop go-clean hugo-clean ## Cleanup project

hugo-clean:  ## Cleanup the content of the directory dist/
	@rm -Rf dist/

go-clean:  ## Stop the application. Delete the binary and the log file:
	@rm -f awesome-api awesome-api.log
	@rm -f coverage-units.out
	@rm -f coverage-integrations.out

check: ## Check syntaxis and links
	@find ./content -name \*.md -print0 | xargs -0 -n1 markdownlint
	@find ./content -name \*.md -print0 | xargs -0 -n1 markdown-link-check

go-lint:   ## Catch errors without the need to compile or execute the application
	@golangci-lint run

validate: ## Validate W3C HTML
	@python3 /opt/W3C-Validator/w3c_validator.py ./dist/index.html 2>&1 || true

lint: check go-lint  ## Staticly analyse the app source code and markdown files

test: unit-tests integration-tests validate  ## Ensure that it behaves as expected

unit-tests:  ## Run unit tests on web server
	@go test -v -short -coverprofile=coverage-units.out

integration-tests: ## Run integration tests on web server
	@go test -v -coverprofile=coverage-integrations.out

help:   ## Show this help usage
	@cat Makefile | grep "^[a-z][a-z\-]*:.*##.*" | sed 's/:.*## /: /'
