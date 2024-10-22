# ASU SHARES FHIR Sandbox Controller

This project provides a turnkey FHIR controller for loading FHIR seed data into a locally running [ASU SHARES](https://www.asushares.com) software stack, including the FHIR bundles used by SHARES publications, presentations, and demos. See [GitHub](https://github.com/asushares/stack) for source code.


## Running the Complete Stack with Pre-Built Images (HIGHLY RECOMMENDED)

You may download and run compelete, pre-configured architectures of the SHARES stack from [HL7 FHIR Foundry](https://foundry.hl7.org), which includes a copy of this controller and FHIR data bundles. Follow the instructions in the SHARES product listing for instructions. You will need Docker Desktop, podman or other compatible container runtime. (If you don't know, start with Docker Desktop.) Foundry will generate a working docker-compose.yml file for you to run without modification.

```sh
docker compose -f docker-compose.yml up --pull always --remove-orphans
```

Once you have the stack running, simply disable the pre-built service you with to work on and replace it with your own locally-modified version.

## Running a Pre-Built Stack Controller Image

If you _only_ want to run a pre-built copy of the stack controller.
```sh
docker run -it --rm -p 4204:80 --pull always asushares/stack:latest
```

The controller UI may be accessed at http://localhost:4204 , however, since you are not using our full distributable stack, you will need to provide your own FHIR R5 server backend prior to loading the FHIR data bundles. The configured FHIR URLs are set to HAPI FHIR defaults, but any R5 server may be used. For example:

```sh
# Run a copy of HAPI or other R5 FHIR server:
docker run -it --rm -p 8080:8080 -e hapi.fhir.fhir_version=R5 -e spring.main.allow-bean-definition-overriding=true hapiproject/hapi:v7.2.0
```


With data loaded, you may now run any of the other SHARES familar of services.

| URL                   | Service           | Purpose       | Source Code   |
|----                   |----               |----           |----           |
| http://localhost:4204 | Stack Controller  | This application, for data (re)loading. Start here. | https://github.com/asushares/stack
| http://localhost:4203 | Patient Portal    | Patient-facing simple consent editor | https://github.com/asushares/patient
| http://localhost:4202 | Rules Editor      | CDS rule editor, if "basic" mode is used. | https://github.com/asushares/rules
| http://localhost:4201 | Consent Manager   | Provider-facing complex consent editor  | https://github.com/asushares/consent-manager
| http://localhost:8080 | FHIR Server       | Underlying FHIR data repository | https://github.com/hapifhir/hapi-fhir
| http://localhost:3000 | CDS Engine        | SHARES CDS Hooks service | https://github.com/asushares/cds


## Building and Running Your Own Custom Build

```sh
# Substitute with your own repository.
docker buildx build -t asushares/stack:latest . 
```

```sh
# Run it on port 4204 (or other of your choice)
docker run -it --rm -p 4204:80 asushares/stack:latest --pull always
```
Open http://localhost:4204 in your browser to use the controller.

## Notes for macOS and ARM Users

TODO

## License

Apache 2.0

## Attribution

Preston Lee
