## katello-publish-content-view
This script publishes a new version of a Katello Content View if the last version of a Content View is not up-to-date.

## Prerequisite
Python 3.6+

## Hot to use
Clone this repo and edit config.yml:
```bash
$ git clone https://github.com/brisa-dev/katello-publish-content-view.git
$ cd katello-publish-content-view
$ vi config.yml
```

Change config.yml with your custom data:
```yaml
envs:
  lab:
    organization_id: 1
    username: some_user
    password: some_pass
    api_url: https://katello.example.io
```

Run script:
```bash
$ ./run.py --env lab # change config.yml with your custom variables
```

Read run.log file to see the output.
```bash
$ cat run.log
```
If all Content View is up-to-date you will receive the message bellow:
```
--- Initializing Katello Content View Checker --- 
It's now       : 2022-06-03 10:39:27 
Content view   : zabbix 
Last Published : 2022-06-02 21:29:17 
Status         : It's not necessary to publish a new version for this Content View 
```
If a new version of a Content View was created you will receive the message bellow:
```
--- Initializing Katello Content View Checker --- 
It's now       : 2022-06-03 10:44:06 
Content view   : zabbix 
Last Published : 2022-06-02 21:29:17 
Status         : It's necessary to publish a new version for this Content View 
Action         : Creating a new version of a Content View... 
Action status  : Success! 
```

## Flow
```mermaid
sequenceDiagram
    run->>get_content_views: Give me the Content View Data
    get_content_views-->>run: return Content View Data
    get_content_views->>get_repository_info: Please, give me the repository data.
    get_repository_info-->>get_content_views: return Repository Data
    loop
      run-->run:Compare dates between Content View and Content View Repositories
      alt If Content View is not up-to-date
          run->>publish_new_content_view: Publish a new version of Content View
      end
    end
```