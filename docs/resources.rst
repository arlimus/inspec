=====================================================
InSpec Resources Reference
=====================================================

The following InSpec resources are available:

* ``apt``
* ``bond``
* ``bridge``
* ``command``
* ``directory``
* ``file``
* ``gem``
* ``group``
* ``host``
* ``interface``
* ``iptables``
* ``kernel_module``
* ``kernel_parameter``
* ``npm``
* ``oneget``
* ``os``
* ``os_env``
* ``package``
* ``pip``
* ``port``
* ``processes`` << process?
* ``registry_key``
* ``script``
* ``service``
* ``user``
* ``windows_feature``
* ``yum``

In addition to the open source resources, Chef Compliance ships with additional resources:

* ``apache_conf``
* ``audit_policy``
* ``audit_daemon_conf`` << auditd_conf?
* ``audit_daemon_rules`` << auditd_rules?
* ``csv``
* ``etc_group``
* ``group_policy``
* ``inetd_config``
* ``json``
* ``limits_conf``
* ``login_defs``
* ``mysql`` << really a resource?
* ``mysql_conf``
* ``mysql_session``
* ``ntp_conf``
* ``parse_config``
* ``parse_config_file``
* ``passwd`` << etc_passwd?
* ``postgres`` << really a resource?
* ``postgres_conf``
* ``postgres_session``
* ``security_policy``
* ``ssh_config``
* ``sshd_config``
* ``yaml``

See below for more information about each InSpec resource, its related matchers, and examples of how to use it in a recipe.


apache_conf -- DONE
=====================================================
Use the ``apache_conf`` InSpec resource to test the configuration settings for |apache|. This file is typically located under ``/etc/apache2`` on the |debian| and |ubuntu| platforms and under ``/etc/httpd`` on the |fedora|, |centos|, |redhat enterprise linux|, and |archlinux| platforms. The configuration settings may vary significantly from platform to platform.

Syntax -- DONE
-----------------------------------------------------
A ``apache_conf`` InSpec resource block declares configuration settings that should be tested:

.. code-block:: ruby

   describe apache_conf('path') do
     its('setting_name') { should eq 'value' }
   end

where

* ``'setting_name'`` is a configuration setting defined in the |apache| configuration file
* ``('path')`` is the non-default path to the |apache| configuration file
* ``{ should eq 'value' }`` is the value that is expected

Matchers -- DONE
-----------------------------------------------------
This InSpec resource matches any service that is listed in the |apache| configuration file:

.. code-block:: ruby

     its('PidFile') { should_not eq '/var/run/httpd.pid' }

or:

.. code-block:: ruby

     its('Timeout') { should eq 300 }

For example:

.. code-block:: ruby

   describe apache_conf do
     its('MaxClients') { should eq 100 }
     its('Listen') { should eq '443'}
   end

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource in a test.

**Test for blocking .htaccess files on CentOS** 

.. code-block:: ruby

   describe apache_conf do
     its('AllowOverride') { should eq 'None' }
   end

**Test ports for SSL** 

.. code-block:: ruby
   
   describe apache_conf do
     its('Listen') { should eq '443'}
   end


apt -- DONE
=====================================================
Use the ``apt`` InSpec resource to verify |apt| repositories on the |debian| and |ubuntu| platforms, and also |ppa| repositories on the |ubuntu| platform.

Syntax -- DONE
-----------------------------------------------------
An ``apt`` InSpec resource block tests the contents of |apt| and |ppa| repositories:

.. code-block:: ruby

   describe apt('path') do
     it { should exist }
     it { should be_enabled }
   end

where

* ``apt('path')`` must specify an |apt| or |ppa| repository
* ``('path')`` may be an ``http://`` address, a ``ppa:`` address, or a short ``repo-name/ppa`` address
* ``exist`` and ``be_enabled`` are a valid matchers for this InSpec resource

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

be_enabled -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_enabled`` matcher tests if a package exists in the repository:

.. code-block:: ruby

   it { should be_enabled }

exist -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``exist`` matcher tests if a package exists on the system:

.. code-block:: ruby

   it { should exist }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource in a test.

**Test if Ubuntu is updated to the latest stable Juju package** 

.. code-block:: ruby

   describe apt('http://ppa.launchpad.net/juju/stable/ubuntu') do
     it { should exist }
     it { should be_enabled }
   end

**Test if Nginx is updated to the latest stable package** 

.. code-block:: ruby

   describe apt('ppa:nginx/stable') do
     it { should exist }
     it { should be_enabled }
   end

**Verify that a repository exists and is enabled**

.. code-block:: ruby

   describe apt('ppa:nginx/stable') do
     it { should exist }
     it { should be_enabled }
   end

**Verify that a repository is not present**

.. code-block:: ruby

   describe apt('ubuntu-wine/ppa') do
     it { should_not exist }
     it { should_not be_enabled }
   end



audit_policy -- DONE
=====================================================
Use the ``audit_policy`` InSpec resource to test auditing policies on the |windows| platform. An auditing policy is a category of security-related events to be audited. Auditing is disabled by default and may be enabled for categories like account management, logon events, policy changes, process tracking, privilege use, system events, or object access. For each auditing category property that is enabled, the auditing level may be set to ``No Auditing``, ``Not Specified``, ``Success``, ``Success and Failure``, or ``Failure``.

Syntax -- DONE
-----------------------------------------------------
A ``audit_policy`` InSpec resource block declares a parameter that belongs to an audit policy category or subcategory:

.. code-block:: ruby

   describe audit_policy do
     its('parameter') { should eq 'value' }
   end

where

* ``'parameter'`` must specify a parameter 
* ``'value'`` must be one of ``No Auditing``, ``Not Specified``, ``Success``, ``Success and Failure``, or ``Failure``

Matchers -- DONE
-----------------------------------------------------
This InSpec resource does not have any matchers.

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test that a parameter is set to "No Auditing"**

.. code-block:: ruby

   describe audit_policy do
     its('Other Account Logon Events') { should_not eq 'No Auditing' }
   end

**Test that a parameter is set to "Success"**

.. code-block:: ruby

   describe audit_policy do
     its('User Account Management') { should_not eq 'No Auditing' }
   end



auditd_conf -- DONE
=====================================================
Use the ``auditd_conf`` InSpec resource to test the configuration settings for the audit daemon. This file is typically located under ``/etc/audit/auditd.conf'`` on |unix| and |linux| platforms.

Syntax -- DONE
-----------------------------------------------------
A ``auditd_conf`` InSpec resource block declares configuration settings that should be tested:

.. code-block:: ruby

   describe auditd_conf('path') do
     its('keyword') { should eq 'value' }
   end

where

* ``'keyword'`` is a configuration setting defined in the ``auditd.conf`` configuration file
* ``('path')`` is the non-default path to the ``auditd.conf`` configuration file
* ``{ should eq 'value' }`` is the value that is expected

Matchers -- DONE
-----------------------------------------------------
This InSpec resource matches any keyword that is listed in the ``auditd.conf`` configuration file:

.. code-block:: ruby

     its('log_format') { should eq 'raw' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test the auditd.conf file** 

.. code-block:: ruby

   describe auditd_conf do
     its('log_file') { should eq '/full/path/to/file' }
     its('log_format') { should eq 'raw' }
     its('flush') { should eq 'none' }
     its('freq') { should eq '1' }
     its('num_logs') { should eq '0' }
     its('max_log_file') { should eq '6' }
     its('max_log_file_action') { should eq 'email' }
     its('space_left') { should eq '2' }
     its('action_mail_acct') { should eq 'root' }
     its('space_left_action') { should eq 'email' }
     its('admin_space_left') { should eq '1' }
     its('admin_space_left_action') { should eq 'halt' }
     its('disk_full_action') { should eq 'halt' }
     its('disk_error_action') { should eq 'halt' }
   end



auditd_rules -- DONE
=====================================================
Use the ``auditd_rules`` InSpec resource to test the rules for logging that exist on the system. The ``audit.rules`` file is typically located under ``/etc/audit/`` and contains the list of rules that define what is captured in log files.

Syntax -- DONE
-----------------------------------------------------
A ``auditd_rules`` InSpec resource block declares one (or more) rules to be tested, and then what that rule should do:

.. code-block:: ruby
   
   describe auditd_rules do
     its('LIST_RULES') { should eq [
      'exit,always syscall=rmdir,unlink',
      'exit,always auid=1001 (0x3e9) syscall=open',
      'exit,always watch=/etc/group perm=wa',
      'exit,always watch=/etc/passwd perm=wa',
      'exit,always watch=/etc/shadow perm=wa',
      'exit,always watch=/etc/sudoers perm=wa',
      'exit,always watch=/etc/secret_directory perm=r',
    ] }
   end

or:

.. code-block:: ruby

   audit = command('/sbin/auditctl -l').stdout
     options = {
       assignment_re: /^\s*([^:]*?)\s*:\s*(.*?)\s*$/,
       multiple_values: true
     }
   
   describe auditd_rules(audit, options) do
     its('rule') { should eq 1 }
   end

where each test

* Must declare one (or more) rules to be tested
* May run a command to ``stdout``, and then run the test against that output
* May use options to define how configuration data is to be parsed

Options -- DONE
-----------------------------------------------------
This InSpec resource supports the following options for parsing configuration data. Use them in an ``options`` block stated outside of (and immediately before) the actual test:

.. code-block:: ruby

   options = {
       assignment_re: /^\s*([^:]*?)\s*:\s*(.*?)\s*$/,
       multiple_values: true
     }
   describe auditd_rules(options) do
     its('rule') { should eq 1 }
   end

assignment_re -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
IDENTICAL TO parse_config << INCLUDE THEM IN BOTH SPOTS WHEN PUBLISHED

multiple_values -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
IDENTICAL TO parse_config << INCLUDE THEM IN BOTH SPOTS WHEN PUBLISHED

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test if a rule contains a matching element that is identified by a regular expression.**

.. code-block:: ruby

   describe audit_daemon_rules do
     its("LIST_RULES") {
       should contain_match(/^exit,always arch=.*
       key=time-change
       syscall=adjtimex,settimeofday/)
     }
   end



bond -- DONE
=====================================================
Use the ``bond`` InSpec resource to test a logical, bonded network interface (i.e. "two or more network interfaces aggregated into a single, logical network interface"). On |unix| and |linux| platforms, any value in the ``/proc/net/bonding`` directory may be tested.

Syntax -- DONE
-----------------------------------------------------
A ``bond`` InSpec resource block declares a bonded network interface, and then specifies the properties of that bonded network interface to be tested:

.. code-block:: ruby

   describe bond('name') do
     it { should exist }
   end

where

* ``'name'`` is the name of the bonded network interface
* ``{ should exist }`` is a valid matcher for this InSpec resource

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

content -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``content`` matcher tests if contents in the file that defines the bonded network interface match the value specified in the test. The values of the ``content`` matcher are arbitrary:

.. code-block:: ruby

   its('content') { should match('value') }

exist -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``exist`` matcher tests if the bonded network interface is available:

.. code-block:: ruby

   it { should exist }

have_interface -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``have_interface`` matcher tests if the bonded network interface has one (or more) secondary interfaces:

.. code-block:: ruby

   it { should have_interface }

interfaces -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``interfaces`` matcher tests if the named secondary interfaces are available:

.. code-block:: ruby

   its('interfaces') { should eq ['eth0', 'eth1', ...] }

params -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``params`` matcher tests arbitrary parameters for the bonded network interface:

.. code-block:: ruby

   its('params') { should eq 'value' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test if eth0 is a secondary interface for bond0** 

.. code-block:: ruby

   describe bond('bond0') do
     it { should exist }
     it { should have_interface 'eth0' }
   end

**Test parameters for bond0** 

.. code-block:: ruby

   describe bond('bond0') do
     its('Bonding Mode') { should eq 'IEEE 802.3ad Dynamic link aggregation' }
     its('Transmit Hash Policy') { should eq 'layer3+4 (1)' }
     its('MII Status') { should eq 'up' }
     its('MII Polling Interval (ms)') { should eq '100' }
     its('Up Delay (ms)') { should eq '0' }
     its('Down Delay (ms)') { should eq '0' }
   end





bridge -- DONE
=====================================================
Use the ``bridge`` InSpec resource to test basic network bridge properties, such as name, if an interface is defined, and the associations for any defined interface.

* On |unix| and |linux| platforms, any value in the ``/sys/class/net/{interface}/bridge`` directory may be tested
* On the |windows| platform, the ``Get-NetAdapter`` cmdlet is associated with the ``Get-NetAdapterBinding`` cmdlet and returns the ``ComponentID ms_bridge`` value as a |json| object

.. not sure the previous two bullet items are actually true, but keeping there for reference for now, just in case

Syntax -- DONE
-----------------------------------------------------
A ``bridge`` InSpec resource block declares xxxxx:

.. code-block:: ruby

   describe bridge('br0') do
     it { should exist }
     it { should have_interface 'eth0' }
   end

.. 
.. where
.. 
.. * ``xxxxx`` must specify xxxxx
.. * xxxxx
.. * ``xxxxx`` is a valid matcher for this InSpec resource
.. 

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

exist -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``exist`` matcher tests if the network bridge is available:

.. code-block:: ruby

   it { should exist }

have_interface -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``have_interface`` matcher tests if the named interface is defined for the network bridge:

.. code-block:: ruby

   it { should have_interface 'eth0' }

interfaces -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``interfaces`` matcher tests if the named interface is present:

.. code-block:: ruby

   its('interfaces') { should eq foo }
   its('interfaces') { should eq bar }
   its('interfaces') { should include foo, bar }

.. wild guessing ^^^

.. 
.. Examples
.. -----------------------------------------------------
.. The following examples show how to use this InSpec resource.
.. 
.. **xxxxx** 
.. 
.. xxxxx
.. 
.. **xxxxx** 
.. 
.. xxxxx
.. 




command -- DONE
=====================================================
Use the ``command`` InSpec resource to test an arbitrary command that is run on the system.

Syntax -- DONE
-----------------------------------------------------
A ``command`` InSpec resource block declares a command to be run, one (or more) expected outputs, and the location to which that output is sent:

.. code-block:: ruby

   describe command('command') do
     it { should exist }
     its('matcher') { should eq 'output' }
   end

or:

.. code-block:: ruby

   describe command('command').exist? do
     its('matcher') { should eq 'output' }
   end

where

* ``'command'`` must specify a command to be run
* ``.exist?`` is the ``exist`` matcher
* ``'matcher'`` is one of ``exit_status``, ``stderr``, or ``stdout``
* ``'output'`` tests the output of the command run on the system versus the output value stated in the test

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

exist -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``exist`` matcher tests if a command may be run on the system:

.. code-block:: ruby

   it { should exist }

exit_status -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``exit_status`` matcher tests the exit status for the command:

.. code-block:: ruby

   its('exit_status') { should eq 123 }

stderr -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``stderr`` matcher tests results of the command as returned in standard error (stderr):

.. code-block:: ruby

   its('stderr') { should eq 'error\n' }

stdout -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``stdout`` matcher tests results of the command as returned in standard output (stdout):

.. code-block:: ruby

   its('stdout') { should eq '/^1$/' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test for PostgreSQL database running a RC, development, or beta release** 

.. code-block:: ruby

   describe command('sudo -i psql -V') do
     its('stdout') { should_not eq '/RC/' }
     its('stdout') { should_not eq '/DEVEL/' }
     its('stdout') { should_not eq '/BETA/' }
   end

**Test for multiple instances of Nginx** 

.. code-block:: ruby

   describe command('ps aux | egrep "nginx: master" | egrep -v "grep" | wc -l') do
     its('stdout') (should eq '/^1$/' )
   end

**Test standard output (stdout)** 

.. code-block:: ruby

   describe command('echo hello') do
     its('stdout') { should eq 'hello\n' }
     its('stderr') { should eq '' }
     its('exit_status') { should eq 0 }
   end

**Test standard error (stderr)** 

.. code-block:: ruby

   describe command('>&2 echo error') do
     its('stdout') { should eq '' }
     its('stderr') { should eq 'error\n' }
     its('exit_status') { should eq 0 }
   end

**Test an exit status code** 

.. code-block:: ruby

   describe command('exit 123') do
     its('stdout') { should eq '' }
     its('stderr') { should eq '' }
     its('exit_status') { should eq 123 }
   end

**Test if the command shell exists** 

.. code-block:: ruby

   describe command('/bin/sh').exist? do
     it { should eq true }
   end

**Test for a command that should not exist** 

.. code-block:: ruby

   describe command('this is not existing').exist? do
     it { should eq false }
   end

**Test for one peer and one indent** 

.. code-block:: ruby

   describe command('sudo -i cat #{hba_config_file} | egrep 'peer|ident' | wc -l') do
     its('stdout') { should eq '(/^[2|1]/)' }
   end

   describe command('sudo -i cat #{hba_config_file} | egrep 'trust|password|crypt' | wc -l') do
     its('stdout') { should eq '(/^0/)' }
   end





csv -- DONE
=====================================================
Use the ``csv`` InSpec resource to test configuration data in a |csv| file.

Syntax -- DONE
-----------------------------------------------------
A ``csv`` InSpec resource block declares the configuration data to be tested:

.. code-block:: ruby

   describe csv('file') do
     its('name') { should eq 'foo' }
   end

where

* ``'file'`` is the path to a |csv| file
* ``name`` is a configuration setting in a |csv| file
* ``should eq 'foo'`` tests a value of ``name`` as read from a |csv| file versus the value declared in the test

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

name -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``name`` matcher tests the value of ``name`` as read from a |csv| file versus the value declared in the test:

.. code-block:: ruby

   its('name') { should eq 'foo' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test a CSV file**

.. code-block:: ruby

   describe csv('some_file.csv') do
     its('setting') { should eq 1 }
   end



directory -- DONE
=====================================================
Use the ``directory`` InSpec resource to test if the file type is a directory. This is equivalent to using the ``file`` InSpec resource and the ``be_directory`` matcher, but provides a simpler and more direct way to test directories. All of the matchers available to the ``file`` resource that may be used with testing directories may be used with this resource.

Syntax -- DONE
-----------------------------------------------------
A ``directory`` InSpec resource block declares the location of the directory to be tested, and then one (or more) matchers:

.. code-block:: ruby

   describe directory('path') do
     it { should MATCHER 'value' }
   end

Matchers -- DONE
-----------------------------------------------------
This InSpec resource may use any of the matchers available to the ``file`` resource that are useful for testing a directory.

.. 
.. Examples
.. -----------------------------------------------------
.. The following examples show how to use this InSpec resource.
.. 
.. **xxxxx** 
.. 
.. xxxxx
.. 
.. **xxxxx** 
.. 
.. xxxxx
.. 


etc_group -- DONE
=====================================================
Use the ``etc_group`` InSpec resource to test groups that are defined on on |linux| and |unix| platforms. The ``/etc/group`` file stores details about each group---group name, password, group identifier, along with a comma-separate list of users that belong to the group.

Syntax -- DONE
-----------------------------------------------------
A ``etc_group`` InSpec resource block declares a collection of properties to be tested:

.. code-block:: ruby

   describe etc_group('path') do
     its('matcher') { should eq 'some_value' }
   end

or:

.. code-block:: ruby

   describe etc_group.where(item: 'value', item: 'value') do
     its('gids') { should_not contain_duplicates }
     its('groups') { should include 'user_name' }
     its('users') { should include 'user_name' }
   end

where

* ``('path')`` is the non-default path to the ``inetd.conf`` file
* ``.where()`` may specify a specific item and value, to which the matchers are compared
* ``'gids'``, ``'groups'``, and ``'users'`` are valid matchers for this InSpec resource

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

gids -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``gids`` matcher tests if the named group identifier is present or if it contains duplicates:

.. code-block:: ruby

     its('gids') { should_not contain_duplicates }

groups -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``groups`` matcher tests all groups for the named user:

.. code-block:: ruby

     its('groups') { should include 'my_user' }

users -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``users`` matcher tests all groups for the named user:

.. code-block:: ruby

     its('users') { should include 'my_user' }

where -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``where`` matcher allows the test to be focused to one (or more) specific items:

.. code-block:: ruby

     etc_group.where(item: 'value', item: 'value')

where ``item`` may be one (or more) of:

* ``name: 'name'``
* ``group_name: 'group_name'``
* ``password: 'password'``
* ``gid: 'gid'``
* ``group_id: 'gid'``
* ``users: 'user_name'``
* ``members: 'member_name'``

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test group identifiers (GIDs) for duplicates** 

.. code-block:: ruby

   describe etc_group do
     its('gids') { should_not contain_duplicates }
   end

**Test all groups to see if a specific user belongs to one (or more) groups** 

.. code-block:: ruby

   describe etc_group do
     its('groups') { should include 'my_user' }
   end


**Test all groups for a specific user name** 

.. code-block:: ruby

   describe etc_group.where(name: 'my_user') do
     its('users') { should include 'my_user' }
   end

**Filter a list of groups for a specific user** 

.. code-block:: ruby

   describe etc_group.where(name: 'my_user') do
     its('users') { should include 'my_user' }
   end



file -- DONE
=====================================================
Use the ``file`` InSpec resource to test all system file types, including files, directories, symbolic links, named pipes, sockets, character devices, block devices, and doors.

Syntax -- DONE
-----------------------------------------------------
A ``file`` InSpec resource block declares the location of the file type to be tested, what type that file should be (if required), and then one (or more) matchers:

.. code-block:: ruby

   describe file('path') do
     it { should MATCHER 'value' }
   end

where

* ``('path')`` is the name of the file and/or the path to the file
* ``MATCHER`` is a valid matcher for this InSpec resource
* ``'value'`` is the value to be tested

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

be_block_device -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_block_device`` matcher tests if the file exists as a block device, such as ``/dev/disk0`` or ``/dev/disk0s9``:

.. code-block:: ruby

   it { should be_block_device }

be_character_device -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_character_device`` matcher tests if the file exists as a character device (that corresponds to a block device), such as ``/dev/rdisk0`` or ``/dev/rdisk0s9``:

.. code-block:: ruby

   it { should be_character_device }

be_directory -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_directory`` matcher tests if the file exists as a directory, such as ``/etc/passwd``, ``/etc/shadow``, or ``/var/log/httpd``:

.. code-block:: ruby

   it { should be_directory }

be_file -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_file`` matcher tests if the file exists as a file. This can be useful with configuration files like ``/etc/passwd`` where there typically is not an associated file extension---``passwd.txt``:

.. code-block:: ruby

   it { should be_file }

be_executable -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_executable`` matcher tests if the file exists as a xxxxx:

.. code-block:: ruby

   it { should be_executable }

.. assuming this carries forward, below -- re: the by owner, group, user examples

The ``be_executable`` matcher may also test if the file is executable by a specific owner, group, or user. For example, a group:

.. code-block:: ruby

   it { should be_executable.by('group') }

an owner:

.. code-block:: ruby

   it { should be_executable.by('owner') }

a user:

.. code-block:: ruby

   it { should be_executable.by_user('user') }

be_grouped_into -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_grouped_into`` matcher tests if the file exists as part of the named group:

.. code-block:: ruby

   it { should be_grouped_into 'group' }

be_immutable -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_immutable`` matcher tests if the file is immutable, i.e. "cannot be changed":

.. code-block:: ruby

   it { should be_immutable }

be_linked_to -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_linked_to`` matcher tests if the file is linked to the named target:

.. code-block:: ruby

   it { should be_linked_to '/etc/target-file' }

be_mounted -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_mounted`` matcher tests if the file is accessible from the file system:

.. code-block:: ruby

   it { should be_mounted }

be_owned_by -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_owned_by`` matcher tests if the file is owned by the named user, such as ``root``:

.. code-block:: ruby

   it { should be_owned_by 'root' }

be_pipe -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_pipe`` matcher tests if the file exists as first-in, first-out special file (``.fifo``) that is typically used to define a named pipe, such as ``/var/log/nginx/access.log.fifo``:

.. code-block:: ruby

   it { should be_pipe }

be_readable -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_readable`` matcher tests if the file is readable:

.. code-block:: ruby

   it { should be_readable }

.. assuming this carries forward, below -- re: the by owner, group, user examples

The ``be_readable`` matcher may also test if the file is readable by a specific owner, group, or user. For example, a group:

.. code-block:: ruby

   it { should be_readable.by('group') }

an owner:

.. code-block:: ruby

   it { should be_readable.by('owner') }

a user:

.. code-block:: ruby

   it { should be_readable.by_user('user') }

be_socket -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_socket`` matcher tests if the file exists as socket (``.sock``), such as ``/var/run/php-fpm.sock``:

.. code-block:: ruby

   it { should be_socket }

be_symlink -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_symlink`` matcher tests if the file exists as a symbolic, or soft link that contains an absolute or relative path reference to another file:

.. code-block:: ruby

   it { should be_symlink }

be_version -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_version`` matcher tests the version of the file:

.. code-block:: ruby

   it { should be_version '1.2.3' }

be_writable -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_writable`` matcher tests if the file is writable:

.. code-block:: ruby

   it { should be_writable }

.. assuming this carries forward, below -- re: the by owner, group, user examples

The ``be_writable`` matcher may also test if the file is writable by a specific owner, group, or user. For example, a group:

.. code-block:: ruby

   it { should be_writable.by('group') }

an owner:

.. code-block:: ruby

   it { should be_writable.by('owner') }

a user:

.. code-block:: ruby

   it { should be_writable.by_user('user') }

content -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``content`` matcher tests if contents in the file match the value specified in the test. The values of the ``content`` matcher are arbitrary and depend on the file type being tested and also the type of information that is expected to be in that file:

.. code-block:: ruby

   its('content') { should contain 'value' }

The following complete example tests the ``pg_hba.conf`` file in |postgresql| for |md5| requirements.  The tests look at all ``host`` and ``local`` settings in that file, and then compare the |md5| checksums against the values in the test:

.. code-block:: bash

   describe file(hba_config_file) do
     its('content') { should eq '/local\s.*?all\s.*?all\s.*?md5/' }
     its('content') { should eq '%r{/host\s.*?all\s.*?all\s.*?127.0.0.1\/32\s.*?md5/}' }
     its('content') { should eq '%r{/host\s.*?all\s.*?all\s.*?::1\/128\s.*?md5/}' }
   end

exist -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``exist`` matcher tests if the named file exists:

.. code-block:: ruby

   it { should exist }

file_version -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``file_version`` matcher tests if the file's version matches the specified value. The difference between a file's "file version" and "product version" is that the file version is the version number of the file itself, whereas the product version is the version number associated with the application from which that file originates:

.. code-block:: ruby

   its('file_version') { should eq '1.2.3' }

group -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``group`` matcher tests if the group to which a file belongs matches the specified value:

.. code-block:: ruby

   its('group') { should eq 'admins' }

have_mode -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``have_mode`` matcher tests if a file has a mode assigned to it:

.. code-block:: ruby

   it { should have_mode }

link_path -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``link_path`` matcher tests if the file exists at the specified path:

.. code-block:: ruby

   its('link_path') { should eq '/some/path/to/file' }

link_target -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``link_target`` matcher tests if a file that is linked to this file exists at the specified path:

.. code-block:: ruby

   its('link_target') { should eq '/some/path/to/file' }

md5sum -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``md5sum`` matcher tests if the |md5| checksum for a file matches the specified value:

.. code-block:: ruby

   its('md5sum') { should eq '3329x3hf9130gjs9jlasf2305mx91s4j' }

mode -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``mode`` matcher tests if the mode assigned to the file matches the specified value:

.. code-block:: ruby

   its('mode') { should eq 0644 }

mtime -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``mtime`` matcher tests if the file modification time for the file matches the specified value:

.. code-block:: ruby

   its('mtime') { should eq 'October 31 2015 12:10:45' }

or:

.. code-block:: bash

   describe file('/').mtime.to_i do
     it { should <= Time.now.to_i }
     it { should >= Time.now.to_i - 1000}
   end

owner -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``owner`` matcher tests if the owner of the file matches the specified value:

.. code-block:: ruby

   its('owner') { should eq 'root' }

product_version -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``product_version`` matcher tests if the file's product version matches the specified value. The difference between a file's "file version" and "product version" is that the file version is the version number of the file itself, whereas the product version is the version number associated with the application from which that file originates:

.. code-block:: ruby

   its('product_version') { should eq 2.3.4 }

selinux_label -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``selinux_label`` matcher tests if the |selinux| label for a file matches the specified value:

.. code-block:: ruby

   its('product_version') { should eq 'system_u:system_r:httpd_t:s0' }


sha256sum -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``sha256sum`` matcher tests if the |sha256| checksum for a file matches the specified value:

.. code-block:: ruby

   its('sha256sum') { should eq 'b837ch38lh19bb8eaopl8jvxwd2e4g58jn9lkho1w3ed9jbkeicalplaad9k0pjn' }
   
size -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``size`` matcher tests if a file's size matches, is greater than, or is less than the specified value. For example, equal:

.. code-block:: ruby

   its('size') { should eq 32375 }

Greater than:

.. code-block:: ruby

   its('size') { should > 64 }

Less than:

.. code-block:: ruby

   its('size') { should < 10240 }

type -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``type`` matcher tests if the first letter of the file's mode string contains one of the following characters:

* ``-`` or ``f`` (the file is a file); use ``'file`` to test for this file type
* ``d`` (the file is a directory); use ``'directory`` to test for this file type
* ``l`` (the file is a symbolic link); use ``'link`` to test for this file type
* ``p`` (the file is a named pipe); use ``'pipe`` to test for this file type
* ``s`` (the file is a socket); use ``'socket`` to test for this file type
* ``c`` (the file is a character device); use ``'character`` to test for this file type
* ``b`` (the file is a block device); use ``'block`` to test for this file type
* ``D`` (the file is a door); use ``'door`` to test for this file type

For example:

.. code-block:: ruby

   its('type') { should eq 'file' }

or:

.. code-block:: ruby

   its('type') { should eq 'socket' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test the contents of a file for MD5 requirements** 

.. code-block:: bash

   describe file(hba_config_file) do
     its('content') { should eq '/local\s.*?all\s.*?all\s.*?md5/' }
     its('content') { should eq '%r{/host\s.*?all\s.*?all\s.*?127.0.0.1\/32\s.*?md5/}' }
     its('content') { should eq '%r{/host\s.*?all\s.*?all\s.*?::1\/128\s.*?md5/}' }
   end

**Test if a file exists** 

.. code-block:: bash

   describe file('/tmp') do
    it { should exist }
   end

**Test that a file does not exist** 

.. code-block:: bash

   describe file('/tmpest') do
    it { should_not exist }
   end

**Test if a file is a directory** 

.. code-block:: bash

   describe file('/tmp') do
    its('type') { should eq :directory }
    it { should be_directory }
   end

**Test if a file is a file and not a directory** 

.. code-block:: bash

   describe file('/proc/version') do
     its('type') { should eq 'file' }
     it { should be_file }
     it { should_not be_directory }
   end

**Test if a file is a symbolic link** 

.. code-block:: bash

   describe file('/dev/stdout') do
     its('type') { should eq 'symlink' }
     it { should be_symlink }
     it { should_not be_file }
     it { should_not be_directory }
   end

**Test if a file is a character device** 

.. code-block:: bash

   describe file('/dev/zero') do
     its('type') { should eq 'character' }
     it { should be_character_device }
     it { should_not be_file }
     it { should_not be_directory }
   end

**Test if a file is a block device** 

.. code-block:: bash

   describe file('/dev/zero') do
     its('type') { should eq 'block' }
     it { should be_character_device }
     it { should_not be_file }
     it { should_not be_directory }
   end

**Test the mode for a file** 

.. code-block:: bash

   describe file('/dev') do
    its('mode') { should eq 00755 }
   end

**Test the owner of a file** 

.. code-block:: bash

   describe file('/root') do
     its('owner') { should eq 'root' }
   end

**Test if a file is owned by the root user** 

.. code-block:: bash

   describe file('/dev') do
     it { should be_owned_by 'root' }
   end

**Test the mtime for a file** 

.. code-block:: bash

   describe file('/').mtime.to_i do
     it { should <= Time.now.to_i }
     it { should >= Time.now.to_i - 1000}
   end

**Test that a file's size is between 64 and 10240** 

.. code-block:: bash

   describe file('/') do
     its('size') { should be > 64 }
     its('size') { should be < 10240 }
   end

**Test that a file's size is zero** 

.. code-block:: bash

   describe file('/proc/cpuinfo') do
     its('size') { should be 0 }
   end

**Test that a file is not mounted** 

.. code-block:: bash

   describe file('/proc/cpuinfo') do
     it { should_not be_mounted }
   end

**Test an MD5 checksum** 

.. code-block:: bash

   require 'digest'
   cpuinfo = file('/proc/cpuinfo').content
   
   md5sum = Digest::MD5.hexdigest(cpuinfo)
   
   describe file('/proc/cpuinfo') do
     its('md5sum') { should eq md5sum }
   end

**Test an SHA-256 checksum** 

.. code-block:: bash

   require 'digest'
   cpuinfo = file('/proc/cpuinfo').content
   
   sha256sum = Digest::SHA256.hexdigest(cpuinfo)
   
   describe file('/proc/cpuinfo') do
     its('sha256sum') { should eq sha256sum }
   end


gem -- DONE
=====================================================
Use the ``gem`` InSpec resource to test if a global |gem| package is installed.

Syntax -- DONE
-----------------------------------------------------
A ``gem`` InSpec resource block declares a package and (optionally) a package version:

.. code-block:: ruby

   describe gem('gem_package_name') do
     it { should be_installed }
   end

where

* ``('gem_package_name')`` must specify a |gem| package, such as ``'rubocop'``
* ``be_installed`` is a valid matcher for this InSpec resource

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

be_installed -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_installed`` matcher tests if the named |gem| package is installed:

.. code-block:: ruby

   it { should be_installed }

version -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``version`` matcher tests if the named package version is on the system:

.. code-block:: ruby

   its('version') { should eq '0.33.0' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Verify that a gem package is installed, with a specific version**

.. code-block:: ruby

   describe gem('rubocop') do
     it { should be_installed }
     its('version') { should eq '0.33.0' }
   end

**Verify that a gem package is not installed**

.. code-block:: ruby

   describe gem('rubocop') do
     it { should_not be_installed }
   end


group -- DONE
=====================================================
Use the ``group`` InSpec resource to test groups on the system.

Syntax -- DONE
-----------------------------------------------------
A ``group`` InSpec resource block declares a group, and then the details to be tested, such as if the group is a local group, the group identifier, or if the group exists:

.. code-block:: ruby

   describe group('group_name') do
     it { should exist }
     its('gid') { should eq 0 }
   end

where

* ``'group_name'`` must specify the name of a group on the system
* ``exist`` and ``'gid'`` are valid matchers for this InSpec resource

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

be_local -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_local`` matcher tests if the group is a local group:

.. code-block:: ruby

   it { should be_local }

exist -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``exist`` matcher tests if the named user exists:

.. code-block:: ruby

   it { should exist }

gid -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``gid`` matcher tests the named group identifier:

.. code-block:: ruby

   its('gid') { should eq 1234 }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test the group identifier for the root group** 

.. code-block:: ruby

   describe group('root') do
     it { should exist }
     its('gid') { should eq 0 }
   end



group_policy -- DONE
=====================================================
Use the ``group_policy`` InSpec resource to test group policy on the |windows| platform. This resource uses the ``Get-Item`` cmdlet to return all of the policy keys and related values.

Syntax -- DONE
-----------------------------------------------------
A ``group_policy`` InSpec resource block declares the path to the policy:

.. code-block:: ruby

   describe group_policy('Path\to\Policy') do
     its('setting') { should eq 'value' }
   end

where

* ``'Path\to\Policy'`` must specify a group policy, such as ``'Local Policies\Audit Policy'`` or ``'Local Policies\Security Options'``
* ``'setting'`` is the group policy setting to be tested
* ``'value'`` is compared to the value on the group policy

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

setting -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``setting`` matcher tests specific, named settings in the group policy:

.. code-block:: ruby

   its('setting') { should eq 'value' }

Use a ``setting`` matcher for each setting to be tested.

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test if users are logged off after the logon time expires** 

.. code-block:: ruby

   describe group_policy('Local Policies\Security Options') do
     its('Automatically log off users when the logon time expires') { should eq 'Enabled' }
   end


host -- DONE
=====================================================
Use the ``host`` InSpec resource to test the name used to refer to a specific host and its availability, including the Internet protocols and ports over which that host name should be available.

Syntax -- DONE
-----------------------------------------------------
A ``host`` InSpec resource block declares a host name, and then (depending on what is to be tested) a port and/or a protocol:

.. code-block:: ruby

   describe host('example.com', port: 80, proto: 'udp') do
     it { should be_reachable }
   end

where

* ``host()`` must specify a host name and may specify a port number and/or a protocol
* ``'example.com'`` is the host name
* ``port:`` is the port number
* ``proto: 'name'`` is the Internet protocol: |icmp| (``proto: 'icmp'``), |tcp| (``proto: 'tcp'``), or |udp| (``proto: 'udp'``)
* ``be_reachable`` is a valid matcher for this InSpec resource

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

be_reachable -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_reachable`` matcher tests if the host name is available:

.. code-block:: ruby

     it { should be_reachable }


be_resolvable -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_resolvable`` matcher tests for host name resolution, i.e. "resolvable to an IP address":

.. code-block:: ruby

     it { should be_resolvable }


ipaddress -- DONE
-----------------------------------------------------
The ``ipaddress`` matcher tests if a host name is resolvable to a specific IP address:

.. code-block:: ruby

     its('ipaddress') { should include '93.184.216.34' }


Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Verify host name s reachable over a specific protocol and port number** 

.. code-block:: ruby

   describe host('example.com', port: 53, proto: 'udp') do
     it { should be_reachable }
   end

**Verify that a specific IP address can be resolved** 

.. code-block:: ruby

   describe host('example.com', port: 80, proto: 'tcp') do
     it { should be_resolvable }
     its('ipaddress') { should include '192.168.1.1' }
   end




inetd_config -- DONE
=====================================================
Use the ``inetd_config`` InSpec resource to test if a service is enabled in the ``inetd.conf`` file on |linux| and |unix| platforms. |inetd|---the Internet service daemon---listens on dedicated ports, and then loads the appropriate program based on a request. The ``inetd.conf`` file is typically located at ``/etc/inetd.conf`` and contains a list of Internet services associated to the ports on which that service will listen. Only enabled services may handle a request; only services that are required by the system should be enabled.

Syntax -- DONE
-----------------------------------------------------
A ``inetd_config`` InSpec resource block declares the list of services that should be disabled in the ``inetd.conf`` file:

.. code-block:: ruby

   describe inetd_config('path') do
     its('service_name') { should eq 'value' }
   end

where

* ``'service_name'`` is a service listed in the ``inetd.conf`` file
* ``('path')`` is the non-default path to the ``inetd.conf`` file
* ``should eq 'value'`` is the value that is expected

Matchers -- DONE
-----------------------------------------------------
This InSpec resource matches any service that is listed in the ``inetd.conf`` file:

.. code-block:: ruby

     its('shell') { should eq nil }

or:

.. code-block:: ruby

     its('netstat') { should eq nil }

or:

.. code-block:: ruby

     its('systat') { should eq nil }

For example:

.. code-block:: ruby

   describe inetd_conf do
     its('shell') { should eq nil }
     its('login') { should eq nil }
     its('exec') { should eq nil }
   end

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Verify that FTP is disabled** 

The contents if the ``inetd.conf`` file contain the following:

.. code-block:: text

   #ftp      stream   tcp   nowait   root   /usr/sbin/tcpd   in.ftpd -l -a
   #telnet   stream   tcp   nowait   root   /usr/sbin/tcpd   in.telnetd

and the following test is defined:

.. code-block:: ruby

   describe inetd_config do
     its('ftp') { should eq nil }
     its('telnet') { should eq nil }
   end

Because both the ``ftp`` and ``telnet`` Internet services are commented out (``#``), both services are disabled. Consequently, both tests will return ``true``. However, if the ``inetd.conf`` file is set as follows:

.. code-block:: text

   ftp       stream   tcp   nowait   root   /usr/sbin/tcpd   in.ftpd -l -a
   #telnet   stream   tcp   nowait   root   /usr/sbin/tcpd   in.telnetd

then the same test will return ``false`` for ``ftp`` and the entire test will fail.

**Test if telnet is installed** 

.. code-block:: ruby

   describe package('telnetd') do
     it { should_not be_installed }
   end
   
   describe inetd_conf do
     its('telnet') { should eq nil }
   end



interface -- DONE
=====================================================
Use the ``interface`` InSpec resource to test basic network adapter properties, such as name, status, state, address, and link speed (in MB/sec).

* On |unix| and |linux| platforms, any value in the ``/sys/class/net/#{iface}`` directory may be tested.
* On the |windows| platform, the ``Get-NetAdapter`` cmdlet returns the following values: ``Property Name``, ``InterfaceDescription``, ``Status``, ``State``, ``MacAddress``, ``LinkSpeed``, ``ReceiveLinkSpeed``, ``TransmitLinkSpeed``, and ``Virtual``, returned as a |json| object.

.. not sure the previous two bullet items are actually true, but keeping there for reference for now, just in case

Syntax -- DONE
-----------------------------------------------------
A ``interface`` InSpec resource block declares network interface properties to be tested:

.. code-block:: ruby

   describe interface do
     it { should be_up }
     its('speed') { should eq 1000 }
     its('name') { should eq eth0 }
   end

.. 
.. where
.. 
.. * ``xxxxx`` must specify xxxxx
.. * xxxxx
.. * ``xxxxx`` is a valid matcher for this InSpec resource
.. 

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

be_up -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_up`` matcher tests if the network interface is available:

.. code-block:: ruby

   it { should be_up }

name -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``name`` matcher tests if the named network interface exists:

.. code-block:: ruby

   its('name') { should eq eth0 }

speed -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``speed`` matcher tests the speed of the network interface, in MB/sec:

.. code-block:: ruby

   its('speed') { should eq 1000 }

.. 
.. Examples
.. -----------------------------------------------------
.. The following examples show how to use this InSpec resource.
.. 
.. **xxxxx** 
.. 
.. xxxxx
.. 
.. **xxxxx** 
.. 
.. xxxxx
.. 



iptables -- DONE
=====================================================
Use the ``iptables`` InSpec resource to test rules that are defined in ``iptables``, which maintains tables of IP packet filtering rules. There may be more than one table. Each table contains one (or more) chains (both built-in and custom). A chain is a list of rules that match packets. When the rule matches, the rule defines what target to assign to the packet.

Syntax -- DONE
-----------------------------------------------------
A ``iptables`` InSpec resource block declares tests for rules in IP tables:

.. code-block:: ruby

   describe iptables(rule:'name', table:'name', chain: 'name') do
     it { should have_rule('RULE') }
   end

where

* ``iptables()`` may specify any combination of ``rule``, ``table``, or ``chain``
* ``rule:'name'`` is the name of a rule that matches a set of packets
* ``table:'name'`` is the packet matching table against which the test is run
* ``chain: 'name'`` is the name of a user-defined chain or one of ``ACCEPT``, ``DROP``, ``QUEUE``, or ``RETURN``
* ``have_rule('RULE')`` tests that rule in the iptables file

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

have_rule -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``have_rule`` matcher tests the named rule against the information in the ``iptables`` file:

.. code-block:: ruby

   it { should have_rule('RULE') }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test if the IP table allows a packet through** 

.. code-block:: ruby

   describe iptables do
     it { should have_rule('-P INPUT ACCEPT') }
   end

**Test if the IP table allows a packet through, for a specific table and chain** 

.. code-block:: ruby

   describe iptables(table:'mangle', chain: 'input') do
     it { should have_rule('-P INPUT ACCEPT') }
   end



json -- DONE
=====================================================
Use the ``json`` InSpec resource to test data in a |json| file.

Syntax -- DONE
-----------------------------------------------------
A ``json`` InSpec resource block declares the data to be tested:

.. code-block:: ruby

   describe json do
     its('name') { should eq 'foo' }
   end

where

* ``name`` is a configuration setting in a |json| file
* ``should eq 'foo'`` tests a value of ``name`` as read from a |json| file versus the value declared in the test

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

name -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``name`` matcher tests the value of ``name`` as read from a |json| file versus the value declared in the test:

.. code-block:: ruby

   its('name') { should eq 'foo' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test a cookbook version in a policyfile.lock.json file**

.. code-block:: ruby

   describe json('policyfile.lock.json') do
     its('cookbook_locks.omnibus.version') { should eq('2.2.0') }
   end



kernel_module -- DONE
=====================================================
Use the ``kernel_module`` InSpec resource to test kernel modules on |linux| platforms. These parameters are located under ``/lib/modules``. Any submodule may be tested using this resource.

Syntax -- DONE
-----------------------------------------------------
A ``kernel_module`` InSpec resource block declares a module name, and then tests if that module is a loadable kernel module:

.. code-block:: ruby

   describe kernel_module('module_name') do
     it { should be_loaded }
   end

where

* ``'module_name'`` must specify a kernel module, such as ``'bridge'``
* ``{ should be_loaded }`` tests if the module is a loadable kernel module

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

be_loaded -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_loaded`` matcher tests if the module is a loadable kernel module:

.. code-block:: ruby

   it { should be_loaded }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test if a module is loaded** 

.. code-block:: ruby

   describe kernel_module('bridge') do
     it { should be_loaded }
   end


kernel_parameter -- DONE
=====================================================
Use the ``kernel_parameter`` InSpec resource to test kernel parameters on |linux| platforms. These parameters are located under ``/proc/sys/net``. Any subdirectory may be tested using this resource.

.. https://www.kernel.org/doc/Documentation/kernel-parameters.txt

Syntax -- DONE
-----------------------------------------------------
A ``kernel_parameter`` InSpec resource block declares a parameter and then a value to be tested:

.. code-block:: ruby

   describe kernel_parameter('path.to.parameter') do
     its('value') { should eq 0 }
   end

where

* ``'path.to.parameter'`` must specify a kernel parameter, such as ``'net.ipv4.conf.all.forwarding'``
* ``{ should eq 0 }`` states the value to be tested

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

value -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``value`` matcher tests the value assigned to the named IP address versus the value declared in the test:

.. code-block:: ruby

   its('value') { should eq 0 }
   
Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test if global forwarding is enabled for an IPv4 address** 

.. code-block:: ruby

   describe kernel_parameter('net.ipv4.conf.all.forwarding') do
     its(:value) { should eq 1 }
   end

**Test if global forwarding is disabled for an IPv6 address** 

.. code-block:: ruby

   describe kernel_parameter('net.ipv6.conf.all.forwarding') do
     its(:value) { should eq 0 }
   end

**Test if an IPv6 address accepts redirects** 

.. code-block:: ruby

   describe kernel_parameter('net.ipv6.conf.interface.accept_redirects') do
     its(:value) { should eq 'true' }
   end


limits_conf -- DONE
=====================================================
Use the ``limits_conf`` InSpec resource to test configuration settings in the ``/etc/security/limits.conf`` file. The ``limits.conf`` defines limits for processes (by user and/or group names) and helps ensure that the system on which those processes are running remains stable. Each process may be assigned a hard or soft limit.

* Soft limits are maintained by the shell and defines the number of file handles (or open files) available to the user or group after login
* Hard limits are maintained by the kernel and defines the maximum number of allowed file handles

Entries in the ``limits.conf`` file are similar to:

.. code-block:: bash

   grantmc     soft   nofile   4096
   grantmc     hard   nofile   63536
   
   ^^^^^^^^^   ^^^^   ^^^^^^   ^^^^^
   domain      type    item    value

Syntax -- DONE
-----------------------------------------------------
A ``limits_conf`` InSpec resource block declares a domain to be tested, along with associated type, item, and value:

.. code-block:: ruby

   describe limits_conf('path') do
     its('domain') { should include ['type', 'item', 'value'] }
     its('domain') { should eq ['type', 'item', 'value'] }
   end

where

* ``('path')`` is the non-default path to the ``inetd.conf`` file
* ``'domain'`` is a user or group name, such as ``grantmc``
* ``'type'`` is either ``hard`` or ``soft``
* ``'item'`` is the item for which limits are defined, such as ``core``, ``nofile``, ``stack``, ``nproc``, ``priority``, or ``maxlogins``
* ``'value'`` is the value associated with the ``item``

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

domain -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``domain`` matcher tests the domain in the ``limits.conf`` file, along with associated type, item, and value:

.. code-block:: ruby

   its('domain') { should include ['type', 'item', 'value'] }

For example:

.. code-block:: ruby

   its('grantmc') { should include ['hard', 'nofile', '63536'] }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test * and ftp limits** 

   describe limits_conf('path') do
     its('*') { should include ['soft', 'core', '0'], ['hard', 'rss', '10000'] }
     its('ftp') { should eq ['hard', 'nproc', '0'] }
   end



login_defs -- DONE
=====================================================
Use the ``login_defs`` InSpec resource to test configuration settings in the ``/etc/login.defs`` file. The ``logins.defs`` file defines site-specific configuration for the shadow password suite on |linux| and |unix| platforms, such as password expiration ranges, minimum/maximum values for automatic selection of user and group identifiers, or the method with which passwords are encrypted.

Syntax -- DONE
-----------------------------------------------------
A ``login_defs`` InSpec resource block declares the ``login.defs`` configuration data to be tested:

.. code-block:: ruby

   describe login_defs do
     its('name') { should include('foo') }
   end

where

* ``name`` is a configuration setting in ``login.defs``
* ``{ should include('foo') }`` tests the value of ``name`` as read from ``login.defs`` versus the value declared in the test

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

name -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``name`` matcher tests the value of ``name`` as read from ``login.defs`` versus the value declared in the test:

.. code-block:: ruby

   its('name') { should eq 'foo' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test password expiration settings** 

.. code-block:: ruby

   describe login_defs do
     its('PASS_MAX_DAYS') { should eq '180' }
     its('PASS_MIN_DAYS') { should eq '1' }
     its('PASS_MIN_LEN') { should eq '15' }
     its('PASS_WARN_AGE') { should eq '30' }
   end

**Test the encryption method** 

.. code-block:: ruby

   describe login_defs do
     its('ENCRYPT_METHOD') { should eq 'SHA512' }
   end

**Test xxxxx** <<< what does this test?

.. code-block:: ruby

   describe login_def do
     its('UMASK') { should eq '077' }
     its('PASS_MAX_DAYS.to_i') { should be <= 90 }
   end


mysql -- NOT AN InSpec resource?
=====================================================
TBD

.. This one seems like it's just loading some mysql information on behalf of the mysql_conf and mysql_session InSpec resources. Right?



mysql_conf -- DONE
=====================================================
Use the ``mysql_conf`` InSpec resource to test the contents of the configuration file for |mysql|, typically located at ``/etc/mysql/<version>/my.cnf``.

Syntax -- DONE
-----------------------------------------------------
A ``mysql_conf`` InSpec resource block declares one (or more) settings in the ``my.cnf`` file, and then compares the setting in the configuration file to the value stated in the test:

.. code-block:: ruby

   describe mysql_conf('path') do
     its('setting') { should eq 'value' }
   end

where

* ``'setting'`` specifies a setting in the ``my.cnf`` file
* ``('path')`` is the non-default path to the ``my.cnf`` file
* ``should eq 'value'`` is the value that is expected

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

setting -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``setting`` matcher tests specific, named settings in the ``my.cnf`` file:

.. code-block:: ruby

   its('setting') { should eq 'value' }

Use a ``setting`` matcher for each setting to be tested.

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test the maximum number of allowed connections** 

.. code-block:: ruby

   describe mysql_conf do
     its('max_connections') { should eq '505' }
     its('max_user_connections') { should eq '500' }
   end

**Test slow query logging** 

.. code-block:: ruby

   describe mysql_conf do
     its('slow_query_log_file') { should eq 'hostname_slow.log' }
     its('slow_query_log') { should eq '0' }
     its('log_queries_not_using_indexes') { should eq '1' }
     its('long_query_time') { should eq '0.5' }
     its('min_examined_row_limit') { should eq '100' }
   end

**Test the port and socket on which MySQL listens** 

.. code-block:: ruby

   describe mysql_conf do
     its('port') { should eq '3306' }
     its('socket') { should eq '/var/run/mysqld/mysql.sock' }
   end

**Test connection and thread variables** 

.. code-block:: ruby

   describe mysql_conf do
     its('port') { should eq '3306' }
     its('socket') { should eq '/var/run/mysqld/mysql.sock' }
     its('max_allowed_packet') { should eq '12M' }
     its('default_storage_engine') { should eq 'InnoDB' }
     its('character_set_server') { should eq 'utf8' }
     its('collation_server') { should eq 'utf8_general_ci' }
     its('max_connections') { should eq '505' }
     its('max_user_connections') { should eq '500' }
     its('thread_cache_size') { should eq '505' }
   end

**Test the safe-user-create parameter** 

.. code-block:: ruby

   describe mysql_conf.params('mysqld') do
     its('safe-user-create') { should eq('1') }
   end
  

mysql_session -- DONE
=====================================================
Use the ``mysql_session`` InSpec resource to test SQL commands run against a |mysql| database.

Syntax -- DONE
-----------------------------------------------------
A ``mysql_session`` InSpec resource block declares the username and password to use for the session, and then the command to be run:

.. code-block:: ruby

   sql = mysql_session('username', 'password')

   sql.describe('QUERY') do
     its('output') { should eq('') }
   end

where

* ``sql = mysql_session`` declares a username and password with permission to run the query
* ``describe('QUERY')`` contains the query to be run
* ``its('output') { should eq('') }`` compares the results of the query against the expected result in the test

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

output -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``output`` matcher tests the results of the query:

.. code-block:: ruby

   its('output') { should eq(/^0/) }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test for matching databases**

.. code-block:: ruby

   sql = mysql_session('my_user','password')

   sql.describe('show databases like \'test\';') do
     its(:stdout) { should_not match(/test/) }
   end




npm -- DONE
=====================================================
Use the ``npm`` InSpec resource to test if a global |npm| package is installed. |npm| is the `the package manager for Javascript packages <https://docs.npmjs.com>`__, such as |bower| and |statsd|.

Syntax -- DONE
-----------------------------------------------------
A ``npm`` InSpec resource block declares a package and (optionally) a package version:

.. code-block:: ruby

   describe gem('npm_package_name') do
     it { should be_installed }
   end

where

* ``('npm_package_name')`` must specify a |npm| package, such as ``'bower'`` or ``'statsd'``
* ``be_installed`` is a valid matcher for this InSpec resource

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

be_installed -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_installed`` matcher tests if the named |gem| package and package version (if specified) is installed:

.. code-block:: ruby

   it { should be_installed }

version -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``version`` matcher tests if the named package version is on the system:

.. code-block:: ruby

   its('version') { should eq '1.2.3' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Verify that bower is installed, with a specific version**

.. code-block:: ruby

   describe npm('bower') do
     it { should be_installed }
     its('version') { should eq '1.4.1' }
   end

**Verify that statsd is not installed**

.. code-block:: ruby

   describe npm('statsd') do
     it { should_not be_installed }
   end


ntp_conf -- DONE
=====================================================
Use the ``ntp_conf`` InSpec resource to test the synchronization settings defined in the ``ntp.conf`` file. This file is typically located at ``/etc/ntp.conf``.

Syntax -- DONE
-----------------------------------------------------
A ``ntp_conf`` InSpec resource block declares the synchronization settings that should be tested:

.. code-block:: ruby

   describe ntp_conf('path') do
     its('setting_name') { should eq 'value' }
   end

where

* ``'setting_name'`` is a synchronization setting defined in the ``ntp.conf`` file
* ``('path')`` is the non-default path to the ``ntp.conf`` file
* ``{ should eq 'value' }`` is the value that is expected

Matchers -- DONE
-----------------------------------------------------
This InSpec resource matches any service that is listed in the ``ntp.conf`` file:

.. code-block:: ruby

     its('server') { should_not eq nil }

or:

.. code-block:: ruby

     its('restrict') { should include '-4 default kod notrap nomodify nopeer noquery'}

For example:

.. code-block:: ruby

   describe ntp_conf do
     its('server') { should_not eq nil }
     its('restrict') { should include '-4 default kod notrap nomodify nopeer noquery'}
   end

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test for clock drift against named servers** 

.. code-block:: ruby

   describe ntp_conf do
     its('driftfile') { should eq '/var/lib/ntp/ntp.drift' }
     its('server') { should eq [
       0.ubuntu.pool.ntp.org,
       1.ubuntu.pool.ntp.org,
       2.ubuntu.pool.ntp.org
     ] }
   end



oneget -- DONE
=====================================================
Use the ``oneget`` InSpec resource to test if the named package and/or package version is installed on the system. This resource uses |oneget|, which is `part of the Windows Management Framework 5.0 and Windows 10 <https://github.com/OneGet/oneget>`__. This resource uses the ``Get-Package`` cmdlet to return all of the package names in the |oneget| repository.

Syntax -- DONE
-----------------------------------------------------
A ``oneget`` InSpec resource block declares a package and (optionally) a package version:

.. code-block:: ruby

   describe oneget('name') do
     it { should be_installed }
   end

where

* ``('name')`` must specify the name of a package, such as ``'VLC'``
* ``be_installed`` is a valid matcher for this InSpec resource

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

be_installed -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_installed`` matcher tests if the named package is installed on the system:

.. code-block:: ruby

   it { should be_installed }

version -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``version`` matcher tests if the named package version is on the system:

.. code-block:: ruby

   its('version') { should eq '1.2.3' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test if VLC is installed** 

.. code-block:: ruby

   describe package('VLC') do
     it { should be_installed }
   end


os -- DONE
=====================================================
Use the ``os`` InSpec resource to test the platform on which the system is running.

Syntax -- DONE
-----------------------------------------------------
A ``os`` InSpec resource block declares the platform to be tested:

.. code-block:: ruby

   describe os do
     it { should eq 'platform' }
   end

where

* ``'platform'`` is one of ``bsd``, ``debian``, ``linux``, ``redhat``, ``solaris``, ``suse``,  ``unix``, or ``windows``

Matchers -- DONE
-----------------------------------------------------
This InSpec resource does not have any matchers.

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test for RedHat** 

.. code-block:: ruby

   describe os do
     it { should eq 'redhat' }
   end

**Test for Ubuntu** 

.. code-block:: ruby

   describe os do
     it { should eq 'debian' }
   end

**Test for Microsoft Windows** 

.. code-block:: ruby

   describe os do
     it { should eq 'windows' }
   end


os_env -- DONE
=====================================================
Use the ``os_env`` InSpec resource to test the environment variables for the platform on which the system is running.

Syntax -- DONE
-----------------------------------------------------
A ``os_env`` InSpec resource block declares xxxxx:

.. code-block:: ruby

   describe os_env('VARIABLE') do
     its('matcher') { should eq 1 }
   end

where

* ``('VARIABLE')`` must specify an environment variable, such as ``PATH``
* ``matcher`` is a valid matcher for this InSpec resource

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

exit_status -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``exit_status`` matcher tests the exit status of the platform environment:

.. code-block:: ruby

   its('exit_status') { should eq 0 }

split -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``split`` matcher tests the delimiter between environment variables:

.. code-block:: ruby

   its('split') { should include ('') }

or:

.. code-block:: ruby

   its('split') { should_not include ('.') }

Use ``-1`` to test for cases where there is a trailing colon (``:``), such as ``dir1::dir2:``:

.. code-block:: ruby

   its('split') { should include ('-1') }

stderr -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``stderr`` matcher tests environment variables after they are output to stderr:

.. code-block:: ruby

   its('stderr') { should include('PWD=/root') }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test the PATH environment variable** 

.. code-block:: ruby

   describe os_env('PATH') do |dirs|
     its('split') { should_not include('') }
     its('split') { should_not include('.') }
   end


package -- DONE
=====================================================
Use the ``package`` InSpec resource to test if the named package and/or package version is installed on the system.

Syntax -- DONE
-----------------------------------------------------
A ``package`` InSpec resource block declares a package and (optionally) a package version:

.. code-block:: ruby

   describe package('name') do
     it { should be_installed }
   end

where

* ``('name')`` must specify the name of a package, such as ``'nginx'``
* ``be_installed`` is a valid matcher for this InSpec resource

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

be_installed -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_installed`` matcher tests if the named package is installed on the system:

.. code-block:: ruby

   it { should be_installed }

version -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``version`` matcher tests if the named package version is on the system:

.. code-block:: ruby

   its('version) { should eq '1.2.3' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test if nginx version 1.9.5 is installed** 

.. code-block:: ruby

   describe package('nginx') do
     it { should be_installed }
     its('version') { should eq 1.9.5 }
   end

**Test that a package is not installed** 

.. code-block:: ruby

   describe package('some_package') do
     it { should_not be_installed }
   end

**Test if telnet is installed** 

.. code-block:: ruby

   describe package('telnetd') do
     it { should_not be_installed }
   end
   
   describe inetd_conf do
     its('telnet') { should eq nil }
   end

**Test if ClamAV (an antivirus engine) is installed and running**

.. code-block:: ruby

   describe package('clamav') do
     it { should be_installed }
     its('version') { should eq '0.98.7' }
   end
   
   describe service('clamd') do
     it { should_not be_enabled }
     it { should_not be_installed }
     it { should_not be_running }
   end


parse_config -- DONE
=====================================================
Use the ``parse_config`` InSpec resource to test arbitrary configuration files, such as testing the results of a regular expression, ensuring that settings are commented out, testing for multiple values, and so on.

Syntax -- DONE
-----------------------------------------------------
A ``parse_config`` InSpec resource block declares the location of the configuration file to be tested, and then which settings in that file are to be tested. Because this InSpec resource relies on arbitrary configuration files, the test itself is often arbitrary and relies on custom |ruby| code:

.. code-block:: ruby

   output = command('some-command').stdout
   
   describe parse_config(output, { data_config_option: value } ) do
     its('setting') { should eq 1 }
   end

or:

.. code-block:: ruby

   audit = command('/sbin/auditctl -l').stdout
     options = {
       assignment_re: /^\s*([^:]*?)\s*:\s*(.*?)\s*$/,
       multiple_values: true
     }
   
   describe parse_config(audit, options) do
     its('setting') { should eq 1 }
   end

where each test

* Must declare the location of the configuration file to be tested
* Must declare one (or more) settings to be tested
* May run a command to ``stdout``, and then run the test against that output
* May use options to define how configuration data is to be parsed

Options -- DONE
-----------------------------------------------------
This InSpec resource supports the following options for parsing configuration data. Use them in an ``options`` block stated outside of (and immediately before) the actual test:

.. code-block:: ruby

   options = {
       assignment_re: /^\s*([^:]*?)\s*:\s*(.*?)\s*$/,
       multiple_values: true
     }
   describe parse_config(options) do
     its('setting') { should eq 1 }
   end

assignment_re -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
Use ``assignment_re`` to test a key value using a regular expression:

.. code-block:: ruby

   'key = value'

may be tested using the following regular expression, which determines assignment from key to value:

.. code-block:: ruby

   assignment_re: /^\s*([^=]*?)\s*=\s*(.*?)\s*$/

comment_char -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
Use ``comment_char`` to test for comments in a configuration file:

.. code-block:: ruby

   comment_char: '#'

key_vals -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
Use ``key_vals`` to test how many values a key contains:

.. code-block:: ruby

   key = a b c

contains three values. To test that value to ensure it only contains one, use:

.. code-block:: ruby

   key_vals: 1

multiple_values -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
Use ``multiple_values`` to test for the presence of multiple key values:

.. code-block:: ruby

   'key = a' and 'key = b'
   params['key'] = ['a', 'b']

or:

.. code-block:: ruby

   'key = a' and 'key = b'
   params['key'] = 'b'

To test if multiple values are present, use:

.. code-block:: ruby

   multiple_values: false

The preceding test will fail with the first example and will pass with the second.

standalone_comments -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
Use ``standalone_comments`` to test for comments in a configuration file and to ensure they are not integrated into the same lines as code:

.. code-block:: ruby

   'key = value # comment'
   params['key'] = 'value'

or:

.. code-block:: ruby

   'key = value # comment'
   params['key'] = 'value # comment'

To test if comments are standalone, use:

.. code-block:: ruby

   standalone_comments: true

The preceding test will fail with the second example and will pass with the first.

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test the expiration time for new account passwords** 

.. code-block:: ruby

   output = command('useradd -D').stdout
   
   describe parse_config(output) do
     its('INACTIVE.to_i') { should be >= 35 }
   end

**Test that bob is a user** 

.. code-block:: ruby

   describe parse_config(data, { multiple_values: true }) do
     its('users') { should include 'bob'}
   end


parse_config_file -- DONE
=====================================================
Use the ``parse_config_file`` InSpec resource to test arbitrary configuration files.

Syntax -- DONE (is this really "identical" to the parse_config syntax?)
-----------------------------------------------------
A ``parse_config_file`` InSpec resource block declares the location of the configuration file to be tested, and then which settings in that file are to be tested. Because this InSpec resource relies on arbitrary configuration files, the test itself is often arbitrary and relies on custom |ruby| code:

.. code-block:: ruby

   output = command('some-command').stdout
   
   describe parse_config_file(output, { data_config_option: value } ) do
     its('setting') { should eq 1 }
   end

or:

.. code-block:: ruby

   audit = command('/sbin/auditctl -l').stdout
     options = {
       assignment_re: /^\s*([^:]*?)\s*:\s*(.*?)\s*$/,
       multiple_values: true
     }
   
   describe parse_config_file(audit, options) do
     its('setting') { should eq 1 }
   end

where each test

* Must declare the location of the configuration file to be tested
* Must declare one (or more) settings to be tested
* May run a command to ``stdout``, and then run the test against that output
* May use options to define how configuration data is to be parsed

.. or is this one more like this?

.. code-block:: ruby

   audit = command('/sbin/auditctl -l').stdout
     options = {
       assignment_re: /^\s*([^:]*?)\s*:\s*(.*?)\s*$/,
       multiple_values: true
     }
   
   describe parse_config_file(audit, options) do
     its('setting') { should eq 1 }
   end

where each test

* Must declare the location of the configuration file to be tested
* Must declare one (or more) settings to be tested
* May run a command to ``stdout``, and then run the test against that output
* May use options to define how configuration data is to be parsed

Options -- DONE
-----------------------------------------------------
This InSpec resource supports the following options for parsing configuration data. Use them in an ``options`` block stated outside of (and immediately before) the actual test:

.. code-block:: ruby

   describe parse_config_file(/path/to/config/file) do
     its('setting') { should eq 1 }
   end

InSpec == inspec (command-line)

assignment_re -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
IDENTICAL TO parse_config << INCLUDE THEM IN BOTH SPOTS WHEN PUBLISHED

comment_char -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
IDENTICAL TO parse_config << INCLUDE THEM IN BOTH SPOTS WHEN PUBLISHED

key_vals -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
IDENTICAL TO parse_config << INCLUDE THEM IN BOTH SPOTS WHEN PUBLISHED

multiple_values -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
IDENTICAL TO parse_config << INCLUDE THEM IN BOTH SPOTS WHEN PUBLISHED

standalone_comments -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
IDENTICAL TO parse_config << INCLUDE THEM IN BOTH SPOTS WHEN PUBLISHED

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test a configuration setting** 

.. code-block:: ruby

   describe parse_config_file('/path/to/file.conf') do
    its('PARAM_X') { should eq 'Y' }
   end

**Use options, and then test a configuration setting** 

.. code-block:: ruby

   describe parse_config_file('/path/to/file.conf', { multiple_values: true }) do
    its('PARAM_X') { should include 'Y' }
   end



passwd -- DONE
=====================================================
Use the ``passwd`` InSpec resource to test the contents of ``/etc/passwd``, which contains the following information for users that may log into the system and/or as users that own running processes. The format for ``/etc/passwd`` includes:

* A username
* The password for that user
* The user identifier (UID) assigned to that user
* The group identifier (GID) assigned to that user
* Additional information about that user
* That user's home directory
* That user's default command shell

defined as a colon-delimited row in the file, one row per user:

.. code-block:: bash

   root:x:1234:5678:additional_info:/home/dir/:/bin/bash

Syntax -- DONE
-----------------------------------------------------
A ``passwd`` InSpec resource block declares one (or more) users and associated user information to be tested:

.. code-block:: ruby

   describe passwd do
     its('matcher') { should eq 0 }
   end

where

* ``count``, ``gids``, ``passwords``, ``uid``, ``uids``, ``username``, ``usernames``, and ``users`` are valid matchers for this InSpec resource

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

count -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``count`` matcher tests the number of times the named user appears in ``/etc/passwd``:

.. code-block:: ruby

   its('count') { should eq 1 }

gids -- ?????
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``gids`` matcher tests if xxxxx:

.. code-block:: ruby

   its('gids') { should eq 1234 }

passwords -- ?????
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``passwords`` matcher tests if xxxxx:

.. code-block:: ruby

   its('passwords') { should eq xxxxx }

uid -- ?????
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``uid`` matcher tests if xxxxx:

.. code-block:: ruby

   its('uid') { should eq xxxxx }

uids -- ?????
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``uids`` matcher tests if xxxxx:

.. code-block:: ruby

   its('uids') { should eq 1 }

username -- ?????
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``username`` matcher tests if xxxxx:

.. code-block:: ruby

   its('username') { should eq 'root' }

usernames -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``usernames`` matcher tests if the usernames in the test match the usernames in ``/etc/passwd``:

.. code-block:: ruby

   its('usernames') { should eq ['root', 'www-data'] }

users -- ?????
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``users`` matcher tests if xxxxx:

.. code-block:: ruby

   its('users') { should eq 'root' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**xxxxx** 

.. code-block:: ruby

   describe passwd do
     its('usernames') { should eq 'root' }
     its('uids') { should eq 1 }
   end

**xxxxx** 

.. code-block:: ruby

   describe passwd.uid(0) do
     its('username') { should eq 'root' }
     its('count') { should eq 1 }
   end



pip -- DONE
=====================================================
Use the ``pip`` InSpec resource to test packages that are installed using the |pip| installer.

Syntax -- DONE
-----------------------------------------------------
A ``pip`` InSpec resource block declares a package and (optionally) a package version:

.. code-block:: ruby

   describe pip('Jinja2') do
     it { should be_installed }
   end

where

* ``'Jinja2'`` is the name of the package
* ``be_installed`` tests to see if the ``Jinja2`` package is installed

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

be_installed -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_installed`` matcher tests if the named package is installed on the system:

.. code-block:: ruby

   it { should be_installed }

version -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``version`` matcher tests if the named package version is on the system:

.. code-block:: ruby

   its('version') { should eq '1.2.3' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test if Jinja2 is installed on the system** 

.. code-block:: ruby

   describe pip('Jinja2') do
     it { should be_installed }
   end

**Test if Jinja2 2.8 is installed on the system** 

.. code-block:: ruby

   describe pip('Jinja2') do
     it { should be_installed }
     its('version') { should eq '2.8' }
   end


port -- DONE
=====================================================
Use the ``port`` InSpec resource to test basic port properties, such as port, process, if it's listening.

Syntax -- DONE
-----------------------------------------------------
A ``port`` InSpec resource block declares a port, and then depending on what needs to be tested, a process, protocol, process identifier, and its state (is it listening?):

.. code-block:: ruby

   describe port(514) do
     it { should be_listening }
     its('process') {should eq 'syslog'}
   end

where the ``syslog`` process is tested to see if it's listening on port 514.

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

be_listening -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_listening`` matcher tests if the port is listening for traffic:

.. code-block:: ruby

   it { should be_listening }

be_listening.with() -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_listening`` matcher can also test if the port is listening for traffic over a specific protocol or on local binding address. Use ``.with()`` to specify a protocol or local binding address. For example, a protocol:

.. code-block:: ruby

   it { should be_listening.with('tcp') }

A local binding address:

   it { should be_listening.with('127.0.0.1:631') }

A protocol and a local binding address:

   it { should be_listening.with('tcp', '127.0.0.1:631') }

pid -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``pid`` matcher tests the process identifier (PID):

.. code-block:: ruby

   its('pid') { should eq '27808' }

process -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``process`` matcher tests if the named process is running on the system:

.. code-block:: ruby

   its('process') { should eq 'syslog' }

protocol -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``protocol`` matcher tests the Internet protocol: |icmp| (``'icmp'``), |tcp| (``'tcp'`` or ``'tcp6'``), or |udp| (``'udp'`` or ``'udp6'``):

.. code-block:: ruby

   its('protocol') { should eq 'tcp' }

or for the |ipv6| protocol:

.. code-block:: ruby

   its('protocol') { should eq 'tcp6' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test port 80, listening with the TCP protocol** 

.. code-block:: ruby

   describe port(80) do
     it { should be_listening }
     its('protocol') {should eq 'tcp'}
   end

**Test port 80, listening with TCP version IPv6 protocol** 

.. code-block:: ruby

   describe port(80) do
     it { should be_listening }
     its('protocol') {should eq 'tcp6'}
   end

**Test ports for SSL, then verify ciphers** 

.. code-block:: ruby

   describe port(80) do
     it { should_not be_listening }
   end
   
   describe port(443) do
     it { should be_listening }
     its('protocol') {should eq 'tcp'}
   end
   
   describe sshd_conf do
     its('Ciphers') { should eq('chacha20-poly1305@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr') }
   end


postgres -- NOT AN InSpec resource?
=====================================================
TBD

.. This one seems like it's just loading some postgresql information on behalf of the postgres_conf and postgres_session InSpec resources. Right?


postgres_conf -- DONE
=====================================================
Use the ``postgres_conf`` InSpec resource to test the contents of the configuration file for |postgresql|, typically located at ``/etc/postgresql/<version>/main/postgresql.conf`` or ``/var/lib/postgres/data/postgresql.conf``, depending on the platform.

Syntax -- DONE
-----------------------------------------------------
A ``postgres_conf`` InSpec resource block declares one (or more) settings in the ``postgresql.conf`` file, and then compares the setting in the configuration file to the value stated in the test:

.. code-block:: ruby

   describe postgres_conf('path') do
     its('setting') { should eq 'value' }
   end

where

* ``'setting'`` specifies a setting in the ``postgresql.conf`` file
* ``('path')`` is the non-default path to the ``postgresql.conf`` file
* ``should eq 'value'`` is the value that is expected

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

setting -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``setting`` matcher tests specific, named settings in the ``postgresql.conf`` file:

.. code-block:: ruby

   its('setting') { should eq 'value' }

Use a ``setting`` matcher for each setting to be tested.

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test the maximum number of allowed client connections** 

.. code-block:: ruby

   describe postgres_conf do
     its('max_connections') { should eq '5' }
   end

**Test system logging** 

.. code-block:: ruby

   describe postgres_conf do
     its('logging_collector') { should eq 'on' }
     its('log_connections') { should eq 'on' }
     its('log_disconnections') { should eq 'on' }
     its('log_duration') { should eq 'on' }
     its('log_hostname') { should eq 'on' }
     its('log_line_prefix') { should eq '%t %u %d %h' }
   end

**Test the port on which PostgreSQL listens** 

.. code-block:: ruby

   describe postgres_conf do
     its('port') { should eq '5432' }
   end

**Test the Unix socket settings** 

.. code-block:: ruby

   describe postgres_conf do
     its('unix_socket_directories') { should eq '.s.PGSQL.5432' }
     its('unix_socket_group') { should eq nil }
     its('unix_socket_permissions') { should eq '0770' }
   end

where ``unix_socket_group`` is set to the |postgresql| default setting (the group to which the server user belongs).



postgres_session -- DONE
=====================================================
Use the ``postgres_session`` InSpec resource to test SQL commands run against a |postgresql| database.

Syntax -- DONE
-----------------------------------------------------
A ``postgres_session`` InSpec resource block declares the username and password to use for the session, and then the command to be run:

.. code-block:: ruby

   sql = postgres_session('username', 'password')

   sql.describe('SELECT * FROM pg_shadow WHERE passwd IS NULL;') do
     its('output') { should eq('') }
   end

where

* ``sql = postgres_session`` declares a username and password with permission to run the query
* ``describe('')`` contains the query to be run
* ``its('output') { should eq('') }`` compares the results of the query against the expected result in the test

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

output -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``output`` matcher tests the results of the query:

.. code-block:: ruby

   its('output') { should eq(/^0/) }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test the PostgreSQL shadow password**

.. code-block:: ruby

   sql = postgres_session('my_user', 'password')

   sql.describe('SELECT * FROM pg_shadow WHERE passwd IS NULL;') do
     its('output') { should eq('') }
   end

**Test for risky database entries** 

.. code-block:: ruby

   sql = postgres_session('my_user', 'password')

   sql.describe('SELECT count (*)
                 FROM pg_language
                 WHERE lanpltrusted = 'f'
                 AND lanname!='internal'
                 AND lanname!='c';') do
     its('output') { should eq(/^0/) }
   end



processes -- DONE
=====================================================
Use the ``processes`` InSpec resource to test properties for programs that are running on the system.

Syntax -- DONE
-----------------------------------------------------
A ``processes`` InSpec resource block declares the name of the process to be tested, and then declares one (or more) property/value pairs:

.. code-block:: ruby

   describe processes('process_name') do
     its('property_name') { should eq 'property_value' }
   end

where

* ``processes('process_name')`` must specify the name of a process that is running on the system
* Multiple properties may be tested; for each property to be tested, use an ``its('property_name')`` statement

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

property_name -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``property_name`` matcher tests the named property for the specified value:

.. code-block:: ruby

   its('property_name') { should eq 'property_value' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test if the list length for the mysqld process is 1** 

.. code-block:: ruby

   describe processes('mysqld') do
     its('list.length') { should eq '1' }
   end

**Test if the init process is owned by the root user** 

.. code-block:: ruby

   describe processes('init') do
     its('user') { should eq 'root' }
   end

**Test if a high-priority process is running** 

.. code-block:: ruby

   describe processes('some_process') do
     its('state') { should eq 'R<' }
   end


registry_key -- DONE
=====================================================
Use the ``registry_key`` InSpec resource to test key values in the |windows| registry.

Syntax -- DONE
-----------------------------------------------------
A ``registry_key`` InSpec resource block declares the item in the |windows| registry, the path to a setting under that item, and then one (or more) name/value pairs to be tested:

.. code-block:: ruby

   describe registry_key('registry_item', 'path\to\key') do
     its('name') { should eq 'value' }
   end

where

* ``'registry_item'`` is a key in the |windows| registry
* ``'path\to\key'`` is the path in the |windows| registry
* ``('name')`` and ``'value'`` represent the name of the key and the value assigned to that key

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

name -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``name`` matcher tests the value for the specified registry setting:

.. code-block:: ruby

   its('name') { should eq 'value' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test the start time for the Schedule service** 

.. code-block:: ruby

   describe registry_key('Task Scheduler','HKEY_LOCAL_MACHINE\...\Schedule') do
     its('Start') { should eq 2 }
   end

where ``'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\Schedule'`` is the full path to the setting.


script -- DONE
=====================================================
Use the ``script`` InSpec resource to test a |powershell| script on the |windows| platform.

.. this one is a bit of a wild guess.

Syntax -- DONE
-----------------------------------------------------
A ``script`` InSpec resource block declares xxxxx:

.. code-block:: ruby

   describe script do
     its('script_name') { should include 'total_wild_guess' }
   end

.. 
.. where
.. 
.. * ``xxxxx`` must specify xxxxx
.. * xxxxx
.. * ``xxxxx`` is a valid matcher for this InSpec resource
.. 

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

script_name -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``script_name`` matcher tests the named script against the value specified by the test:

.. code-block:: ruby

   its('script_name') { should include 'total_wild_guess' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

.. stoopid test below; probably need a better one

**Test that user Grantmc belongs to the Active Directory object** 

.. code-block:: ruby

   describe script do
     its('ADObject') { should include 'Get-ADPermission -Identity Grantmc' }
   end



security_policy -- DONE
=====================================================
Use the ``security_policy`` InSpec resource to test security policies on the |windows| platform.

Syntax -- DONE
-----------------------------------------------------
A ``security_policy`` InSpec resource block declares the name of a security policy and the value to be tested:

.. code-block:: ruby

   describe security_policy do
     its('policy_name') { should eq 'value' }
   end

where

* ``'policy_name'`` must specify a security policy
* ``{ should eq 'value' }`` tests the value of ``policy_name`` against the value declared in the test

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

policy_name -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``policy_name`` matcher must be the name of a security policy:

.. code-block:: ruby

   its('SeNetworkLogonRight') { should eq '*S-1-5-11' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Verify that only the Administrators group has remote access**

.. code-block:: ruby

   describe security_policy do
     its('SeRemoteInteractiveLogonRight') { should eq '*S-1-5-32-544' }
   end


service -- DONE
=====================================================
Use the ``service`` InSpec resource to test if the named service is installed, running and/or enabled.

Syntax -- DONE
-----------------------------------------------------
A ``service`` InSpec resource block declares the name of a service and then one (or more) matchers to test the state of the service:

.. code-block:: ruby

   describe service('service_name') do
     it { should be_installed }
     it { should be_enabled }
     it { should be_running }
   end

where

* ``('service_name')`` must specify a service name
* ``be_installed``, ``be_enabled``, and ``be_running`` are valid matchers for this InSpec resource

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

be_enabled -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_enabled`` matcher tests if the named service is enabled:

.. code-block:: ruby

   it { should be_enabled }

be_installed -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_installed`` matcher tests if the named service is installed:

.. code-block:: ruby

   it { should be_installed }

be_running -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_running`` matcher tests if the named service is running:

.. code-block:: ruby

   it { should be_running }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test if the postgresql service is both running and enabled** 

.. code-block:: ruby

   describe service('postgresql') do
     it { should be_enabled }
     it { should be_running }
   end

**Test if the mysql service is both running and enabled** 

.. code-block:: ruby

   describe service('mysqld') do
     it { should be_enabled }
     it { should be_running }
   end

**Test if ClamAV (an antivirus engine) is installed and running**

.. code-block:: ruby

   describe package('clamav') do
     it { should be_installed }
     its('version') { should eq '0.98.7' }
   end
   
   describe service('clamd') do
     it { should_not be_enabled }
     it { should_not be_installed }
     it { should_not be_running }
   end


ssh_config -- DONE
=====================================================
Use the ``ssh_config`` InSpec resource to test |openssh| |ssh| client configuration data located at ``etc/ssh/ssh_config`` on |linux| and |unix| platforms.

Syntax -- DONE
-----------------------------------------------------
A ``ssh_config`` InSpec resource block declares the client |openssh| configuration data to be tested:

.. code-block:: ruby

   describe ssh_config('path') do
     its('name') { should include('foo') }
   end

where

* ``name`` is a configuration setting in ``ssh_config``
* ``('path')`` is the non-default ``/path/to/ssh_config``
* ``{ should include('foo') }`` tests the value of ``name`` as read from ``ssh_config`` versus the value declared in the test 

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

name -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``name`` matcher tests the value of ``name`` as read from ``ssh_config`` versus the value declared in the test:

.. code-block:: ruby

   its('name') { should eq 'foo' }

or:

.. code-block:: ruby

   it's('name') { should include('bar') }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test SSH configuration settings**

.. code-block:: ruby

   describe ssh_config do
     its('cipher') { should eq '3des' }
     its('port') { should '22' }
     its('hostname') { should include('example.com') }
   end

**Test which variables from the local environment are sent to the server**

.. code-block:: ruby

   return unless command('ssh').exist?
   
   describe ssh_config do
     its('SendEnv') { should include('GORDON_CLIENT') }
   end

**Test owner and group permissions**

.. code-block:: ruby

  describe ssh_config do
    its('owner') { should eq 'root' }
    its('mode') { should eq 644 }
  end

**Test SSH configuration**

.. code-block:: ruby

  describe ssh_config do
    its('Host') { should eq '*' }
    its('Tunnel') { should eq nil }
    its('SendEnv') { should eq 'LANG LC_*' }
    its('HashKnownHosts') { should eq 'yes' }
  end


sshd_config -- DONE
=====================================================
Use the ``sshd_config`` InSpec resource to test configuration data for the |openssh| daemon located at ``etc/ssh/sshd_config`` on |linux| and |unix| platforms. sshd---the |openssh| daemon---listens on dedicated ports, starts a daemon for each incoming connection, and then handles encryption, authentication, key exchanges, command executation, and data exchanges.

Syntax -- DONE
-----------------------------------------------------
A ``sshd_config`` InSpec resource block declares the client |openssh| configuration data to be tested:

.. code-block:: ruby

   describe sshd_config('path') do
     its('name') { should include('foo') }
   end

where

* ``name`` is a configuration setting in ``sshd_config``
* ``('path')`` is the non-default ``/path/to/sshd_config``
* ``{ should include('foo') }`` tests the value of ``name`` as read from ``sshd_config`` versus the value declared in the test 

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

name -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``name`` matcher tests the value of ``name`` as read from ``sshd_config`` versus the value declared in the test:

.. code-block:: ruby

   its('name') { should eq 'foo' }

or:

.. code-block:: ruby

   it's('name') {should include('bar') }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test which variables may be sent to the server**

.. code-block:: ruby

   return unless command('sshd').exist?
   
   describe sshd_config do
     its('AcceptEnv') { should include('GORDON_SERVER') }
   end

**Test for IPv6-only addresses**

.. code-block:: ruby

   return unless command('sshd').exist?
   
   describe sshd_config do
     its('AddressFamily') { should eq 'inet6' }
   end

**Test protocols**

.. code-block:: ruby

   describe sshd_config do
     its('Protocol') { should eq '2' }
   end

**Test ports for SSL, then verify ciphers** 

.. code-block:: ruby

   describe port(80) do
     it { should_not be_listening }
   end
   
   describe port(443) do
     it { should be_listening }
     its('protocol') {should eq 'tcp'}
   end
   
   describe sshd_conf do
     its('Ciphers') { should eq('chacha20-poly1305@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr') }
   end

**Test SSH protocols**

.. code-block:: ruby

  describe sshd_config do
    its('Port') { should eq '22' }
    its('UsePAM') { should eq 'yes' }
    its('ListenAddress') { should eq nil }
    its('HostKey') { should eq [
        '/etc/ssh/ssh_host_rsa_key',
        '/etc/ssh/ssh_host_dsa_key',
        '/etc/ssh/ssh_host_ecdsa_key',
      ] }
  end


user -- DONE
=====================================================
Use the ``user`` InSpec resource to test user profiles, including the groups to which they belong, the frequency of required password changes, the directory paths to home and shell.

Syntax -- DONE
-----------------------------------------------------
A ``user`` InSpec resource block declares a user name, and then one (or more) matchers:

.. code-block:: ruby

   describe user('root') do
     it { should exist }
     its('uid') { should eq 1234 }
     its('gid') { should eq 1234 }
     its('group') { should eq 'root' }
     its('groups') { should eq ['root', 'other']}
     its('home') { should eq '/root' }
     its('shell') { should eq '/bin/bash' }
     its('mindays') { should eq 0 }
     its('maxdays') { should eq 90 }
     its('warndays') { should eq 8 }
   end

where

* ``('root')`` is the user to be tested
* ``it { should exist }`` tests if the user exists
* ``gid``, ``group``, ``groups``, ``home``, ``maxdays``, ``mindays``, ``shell``, ``uid``, and ``warndays`` are valid matchers for this InSpec resource

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

exist -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``exist`` matcher tests if the named user exists:

.. code-block:: ruby

   it { should exist }

gid -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``gid`` matcher tests the group identifier:

.. code-block:: ruby

   its('gid') { should eq 1234 } }

where ``1234`` represents the user identifier.

group -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``group`` matcher tests the group to which the user belongs:

.. code-block:: ruby

   its('group') { should eq 'root' }

where ``root`` represents the group.

groups -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``groups`` matcher tests two (or more) groups to which the user belongs:

.. code-block:: ruby

   its('groups') { should eq ['root', 'other']}

home -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``home`` matcher tests the home directory path for the user:

.. code-block:: ruby

   its('home') { should eq '/root' }

maxdays -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``maxdays`` matcher tests the maximum number of days between password changes:

.. code-block:: ruby

   its('maxdays') { should eq 99 }

where ``99`` represents the maximum number of days.

mindays -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``mindays`` matcher tests the minimum number of days between password changes:

.. code-block:: ruby

   its('mindays') { should eq 0 }

where ``0`` represents the maximum number of days.

shell -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``shell`` matcher tests the path to the default shell for the user:

.. code-block:: ruby

   its('shell') { should eq '/bin/bash' }

uid -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``uid`` matcher tests the user identifier:

.. code-block:: ruby

   its('uid') { should eq 1234 } }

where ``1234`` represents the user identifier.

warndays -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``warndays`` matcher tests the number of days a user is warned before a password must be changed:

.. code-block:: ruby

   its('warndays') { should eq 5 }

where ``5`` represents the number of days a user is warned.

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Verify available users for the MySQL server**

.. code-block:: ruby

   describe user('root') do
     it { should exist }
     it { should belong_to_group 'root' }
     its('uid') { should eq 0 }
     its('groups') { should eq ['root'] }
   end
   
   describe user('mysql') do
    it { should_not exist }
   end

**Test users on multiple platforms**

The |nginx| user is typically ``www-data``, but on |centos| it's ``nginx``. The following example shows how to test for the |nginx| user with a single test, but accounting for all platforms:

.. code-block:: ruby

   web_user = 'www-data'
   web_user = 'nginx' if os[:family] == 'centos'
   
   describe user(web_user) do
     it { should exist }
   end


windows_feature -- DONE
=====================================================
Use the ``windows_feature`` InSpec resource to test features on |windows|. The ``Get-WindowsFeature`` cmdlet returns the following values: ``Property Name``, ``DisplayName``, ``Description``, ``Installed``, and ``InstallState``, returned as a |json| object similar to:

.. code-block:: javascript

   {
     "Name": "XPS-Viewer",
     "DisplayName": "XPS Viewer",
     "Description": "The XPS Viewer reads, sets permissions, and digitally signs XPS documents.",
     "Installed": false,
     "InstallState": 0
   }

Syntax -- DONE
-----------------------------------------------------
A ``windows_feature`` InSpec resource block declares the name of the |windows| feature, tests if that feature is installed, and then returns information about that feature:

.. code-block:: ruby

   describe windows_feature('feature_name') do
     it { should be_installed }
   end

where

* ``('feature_name')`` must specify a |windows| feature name, such as ``DHCP Server`` or ``IIS-Webserver``
* ``be_installed`` is a valid matcher for this InSpec resource

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

be_installed -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_installed`` matcher tests if the named |windows| feature is installed:

.. code-block:: ruby

   it { should be_installed }

If the feature is installed, the ``Get-WindowsFeature`` cmdlet is run and the name, display name, description, and install state is returned as a |json| object.

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test the DHCP Server feature**

.. code-block:: ruby

   describe windows_feature('DHCP Server') do
     it{ should be_installed }
   end


yaml -- DONE
=====================================================
Use the ``yaml`` InSpec resource to test configuration data in a |yaml| file.

Syntax -- DONE
-----------------------------------------------------
A ``yaml`` InSpec resource block declares the configuration data to be tested:

.. code-block:: ruby

   describe yaml do
     its('name') { should eq 'foo' }
   end

where

* ``name`` is a configuration setting in a |yaml| file
* ``should eq 'foo'`` tests a value of ``name`` as read from a |yaml| file versus the value declared in the test

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

name -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``name`` matcher tests the value of ``name`` as read from a |yaml| file versus the value declared in the test:

.. code-block:: ruby

   its('name') { should eq 'foo' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test a kitchen.yml file driver**

.. code-block:: ruby

   describe yaml('.kitchen.yaml') do
     its('driver.name') { should eq('vagrant') }
   end


yum -- DONE
=====================================================
Use the ``yum`` InSpec resource to test packages in the |yum| repository.

Syntax -- DONE
-----------------------------------------------------
A ``yum`` InSpec resource block declares a package repo, tests if the package repository is present, and if it that package repository is a valid package source (i.e. "is enabled"):

.. code-block:: ruby

   describe yum.repo('name') do
     it { should exist }
     it { should be_enabled }
   end

where

* ``repo('name')`` is the (optional) name of a package repo, using either a full identifier (``'updates/7/x86_64'``) or a short identifier (``'updates'``)

Matchers -- DONE
-----------------------------------------------------
This InSpec resource has the following matchers.

be_enabled -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``be_enabled`` matcher tests if the package repository is a valid package source:

.. code-block:: ruby

   it { should be_enabled }

exist -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``exist`` matcher tests if the package repository exists:

.. code-block:: ruby

   it { should exist }

repo('name') -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``repo('name')`` matcher names a specific package repository:

.. code-block:: ruby

   describe yum.repo('epel') do
     ...
   end

repos -- DONE
+++++++++++++++++++++++++++++++++++++++++++++++++++++
The ``repos`` matcher tests if a named repo, using either a full identifier (``'updates/7/x86_64'``) or a short identifier (``'updates'``), is included in the |yum| repo:

.. code-block:: ruby

   its('repos') { should include 'some_repo' }

Examples -- DONE
-----------------------------------------------------
The following examples show how to use this InSpec resource.

**Test if the yum repo exists**

.. code-block:: ruby

   describe yum do
     its('repos') { should exist }
   end

**Test if the 'base/7/x86_64' repo exists and is enabled**

.. code-block:: ruby

   describe yum do
     its('repos') { should include 'base/7/x86_64' }
     its('epel') { should exist }
     its('epel') { should be_enabled }
   end

**Test if a specific yum repo exists**

.. code-block:: ruby

   describe yum.repo('epel') do
     it { should exist }
     it { should be_enabled }
   end
