# ansible-ios-acl
Role and an example of playbook to apply access-lists on Cisco IOS devices, just populating a txt file with acl content.<br>
The network operators can easily update the acl file(s) content, without worrying about ansible and yaml syntax.
<br>

The role "ios-acl" will read the "acl-name.txt" file content, and create an access-list in the target device(s) defined in the playbook.

After apply the access-list(s), a debug command returns the access-list(s) applied with "show ip access-lists access-list-name" command.
 
 Example:
 
```sh 
TASK [ios-acl : debug] *********************************************************************************************
ok: [x.x.x.x] => 
    "output.stdout_lines": [
        [
            "Extended IP access list acl-test", 
            "    10 permit tcp 10.0.0.0 0.0.0.255 host 10.100.0.3 eq 443, 
            "    20 permit udp 10.0.0.0 0.0.0.255 host 10.100.0.4 eq 53", 
            "    30 permit tcp 10.0.0.0 0.0.0.255 host 10.100.0.4 eq 53", 
            "    40 permit icmp host 10.0.0.10 host 10.10.10.10", 
            "    1000 deny ip any any"  
```
<br>

# Variables

You must define the variable "acl_name" in the playbook.

**acl_name:** "name-of-acl-in-the-device-config"

Example:

**acl_name:** acl-test

This is to create an access-list named "acl-test" with "acl-test.txt" file content.


<br>

# Running Playbooks:

```sh
$ ansible-playbook -i hosts playbook.yaml --ask-pass
```


You can create more than one acl in the same device(s) just replicating the task content and changing the "acl_name" variable.
You need also to create a file "acl-name.txt" with acl content.

Example:

```sh
 - name: update access-list "{{ acl_name }}"
    vars:
      **acl_name: acl-test-2**
    import_role:
      name: ios-acl
      
  - name: update access-list "{{ acl_name }}"
    vars:
      **acl_name: acl-test-3**
    import_role:
      name: ios-acl
 
 ```

