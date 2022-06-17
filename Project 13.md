# Project 13

## Ansible Dynamic Assignments(include) and Community Roles 

In this project, we will introduce dynamic assignments by using include module.

From Project 12, we know that static assignments use import Ansible module. The module that enables dynamic assignments is include.

For this project, Ansible dynamic assignments (include) will be utilised to deploy to servers.

When the import module is used, all statements are pre-processed at the time playbooks are parsed. Meaning, when you execute site.yml playbook, Ansible will process all the playbooks referenced during the time it is parsing the statements. This also means that, during actual execution, if any statement changes, such statements will not be considered. Hence, it is static.

On the other hand, when include module is used, all statements are processed only during execution of the playbook. Meaning, after the statements are parsed, any changes to the statements encountered during execution will be used.

```
import = Static
include = Dynamic
```


Take note that in most cases it is recommended to use static assignments for playbooks, because it is more reliable.With dynamic ones, it is hard to debug playbook problems due to its dynamic nature.

However, you can use dynamic assignments for environment specific variables as we will be introducing in this project.

### Step 1 - Introducing Dynamic Assignments into our structure


In my github repository https://github.com/opeyemi24/ansible-config-mgt, I started a new branch called dynamic-assignments.

     git checkout -b dynamic-assignments


![capture dynamic assignment](https://user-images.githubusercontent.com/92916632/174153390-a19940d9-7d5e-41fb-affc-c6d082ef8d18.PNG)





