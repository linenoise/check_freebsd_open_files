NAME
    check_freebsd_open_files - Nagios plugin to report on FreeBSD open files

DESCRIPTION
    This script acts as a plugin module for the Nagios IT infrastructure
    monitoring system. It on a FreeBSD server to pull status information on
    open files, allowing queries based on file descriptor, mode, mount,
    process, and user:

    *   descriptor (tr, wd, jail, text, root, mmap)

        This option will filter by the process file table entry special
        descriptor of the opened file as listed in the fstat(1) man page:

                jail    - jail root directory
                mmap    - memory-mapped file
                root    - root inode
                text    - executable text inode
                tr      - kernel trace file
                wd      - current working directory

    *   mode

        This option will filter by the mode of the opened file: 'w' for
        write, 'r' for read, and 'rw' for read and write.

    *   mount

        This option will filter by the system mount on which the file is
        open, such as "-f mount:/usr"

    *   process

        This option will filter by the process that opened the file, such as
        "-f process:httpd"

    *   user

        This option will filter by the username owning the process that
        opened the file, such as "-f user:root".

    This has been tested with FreeBSD 7.1.

SYNOPSIS
  Command Line Interface
    Get the total number of open files:

            check_freebsd_open_files -w 800 -c 1000

    Get the number of files opened by a given user:

            check_freebsd_open_files -w 800 -c 1000 -f user:root

    Get the number of files opened by a process name:

            check_freebsd_open_files -w 800 -c 1000 -f process:httpd

    Get the number of files opened with command line options stored in a
    configuration file:

            check_freebsd_open_files --extra-opts=my_config.ini

  Running within Nagios
    In objects/commands.cfg:

            define command{
                command_name    check_freebsd_open_files
                command_line    $USER1$/check_freebsd_open_files -c $ARG1$ -w $ARG2$
            }

            define command{
                command_name    check_freebsd_open_files_filtered
                command_line    $USER1$/check_freebsd_open_files -c $ARG1$ -w $ARG2$ -f $ARG3$
            }

    In the configuration file for your host running FreeBSD and either
    Nagios or NPRE:

            define service{
                use                   local-service
                host_name             your.hostname.com
                service_description   FreeBSD Open Files
                check_command         check_freebsd_open_files!800!1000
            }

            define service{
                use                   local-service
                host_name             your.hostname.com
                service_description   FreeBSD Open Files by Root
                check_command         check_freebsd_open_files_filtered!800!1000!user:root
            }

            define service{
                use                   local-service
                host_name             your.hostname.com
                service_description   FreeBSD Open Files by HTTPD
                check_command         check_freebsd_open_files_filtered!800!1000!process:httpd
            }

SEE ALSO
    If using an external configuration file, it should be structured
    according to the specification at
    <http://nagiosplugins.org/extra-opts/>.

    Thresholds given to this script should be in the format specified at
    <http://nagiosplug.sourceforge.net/developer-guidelines.html>.

    This module is built upon Nagios::Plugin by the Nagios Plugin
    Development Team. Further reading on Nagios, NPRE, and Nagios Plugins is
    available at <http://nagios.com/>.

AUTHOR
    This script is written and maintained by Danne Stayskal
    <danne@stayskal.com> and is available on his website, at
    <http://danne.stayskal.com/software/check_freebsd/>.

LICENSE
    Copyright (C) 2009 by Danne Stayskal.

    This program is free software; you can redistribute it and/or modify it
    under the terms of the GNU General Public License as published by the
    Free Software Foundation; either version 2 of the License, or (at your
    option) any later version.

    This program is distributed in the hope that it will be useful, but
    WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General
    Public License for more details.

    You should have received a copy of the GNU General Public License along
    with this program; if not, write to the Free Software Foundation, Inc.,
    59 Temple Place, Suite 330, Boston, MA 02111-1307 USA

    Nagios, the Nagios logo, and Nagios graphics are the servicemarks,
    trademarks, or registered trademarks owned by Nagios Enterprises. All
    other servicemarks and trademarks are the property of their respective
    owner.

