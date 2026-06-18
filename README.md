# Guard Dog - The monitoring service for all your need

With this programm you will be able to track the health of programms and hardware

## Description

You have written a cool new project or want to make sure your pi or any other device is running properly?
Then simply add them via CLI and watch the statics be delivered in real time with email alerts if you so wish.

## Getting started

### Prerequisites

- a rasberry pi or any other server
- a piece of soft- or hardware to be monitored

### Quickstart

```bash
    git clone git@github.com:NewJhez01/guard_dog.github
    cd guard_dog
    docker compose up -d
    docker compose run --rm gd:add:{service_type} --n={name}        # add service to be tracked with name flag for a custom name
    docker compose run --rm gd:mail                                 # set up or change the mail address for alerts to be sent to
```

The service types currently availible are

- docker for docker programms
- hware for hardware such as rasberry pi
- tcp for tcp connections
- udp for udp connections

The monitoring will then continuously monitor any programm, hardware or connection to see if they are still up
or if they have unexpectactly stopped.
With the command

```bash
    docker compose run --rm gd:conf:{service_id} # retuned by the add command
```

you can edit set up additionally metrics for alerts such as response time of the tcp udp or cpu usage from the hardware component

## Useful commands

```bash
    docker compose run --rm gd:add:{service_type} --n={name}                # add new service view [quickstart](#quickstart) for more details
    docker compose run --rm gd:ls                                           # list all services with name and id
    docker compose run --rm gd:ls --v                                       # to receive additional info such as extra metrics set
    docker compose run --rm gd:rm                                           # to remove a service not to be tracked
    docker compose run --rm gd:mail                                         # set up or change the mail address for alerts to be sent to
```

```
    cmd/
        main.go                         # wire deps
        server/
            server.go                   # start server for tracking app
        cli/
            cli.go                      # start cli tool
    internal/
        cli/                            # cli tool logic
            handler/
                cli_handler.go          # handle user input and output
            domain/
                create_servie.go        # create a new service to be tracked
                delete_service.go       # delete a service
                list_service.go         # list a service
            repo/
                cli_sqlite_repo.go      # CRUD operations for cli
                cli_repo.go             # interfaces
            infrastructure/
                input_parser.go         # parser user input
                output_parser.go        # parses output for user
        tracker/                        # polling logic for services
            handler/
                tracker_handler.go      # start polling logic and handle result
            domain/
                polling.go              # start polling logic handle peaks
            repo/
                tracker_sqlite_repo.go  # CRUD operations for tracker
                tracker_repo.go         # interfaces
            infrastructure/             # utiliy funcs with no core logic
        mail/                           # mail sending logic
            handler/
                mail_handler.go         # start mail sending process
            domain/
                error_mail.go           # orchestrate error mail process
                update_mail.go          # orchestrate status mail process
            repo/
                mail_smtp_repo.go       # send mail
                mail_repo.go            # interfaces
            infrastructure/
                mail_parser.go
        alert/ # alert eval logic
            handler/
                alert_handler.go        # start alert handling
            domain/
                alert.go                # build alert
```

## Contributing

Found a bug? Have ideas? Check out our [open issues](https://github.com/NewJhez01/guard_dog/issues) or feel free to open a new one. We welcome all contributions!
