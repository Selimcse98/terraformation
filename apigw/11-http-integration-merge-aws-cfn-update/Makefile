.PHONY: help init clean validate mock create delete info deploy
.DEFAULT_GOAL := run

environment = "dnvriend/dev/eu-west-1"

help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

create: ## create env
	@sceptre launch-env $(environment)

delete: ## delete env
	@sceptre delete-env $(environment)

info: ## describe resources
	@sceptre describe-env-resources $(environment)
	@sceptre describe-stack-outputs $(environment) api

merge: ## merge the open-api spec with the api-gateway-extensions to a CloudFormation template
	@aws-cfn-update \
		--verbose rest-api-body  \
		--resource ApiGatewayApi \
		--open-api-specification swagger/swagger.yaml \
		--api-gateway-extensions swagger/aws-extensions.yaml \
		templates/api.yaml

deploy: delete merge create info ## delete and create
