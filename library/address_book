#!/usr/bin/env python
# -*- coding: utf-8 -*-

# Copyright 2014 Spotify AB. All rights reserved.
#
# The contents of this file are licensed under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with the
# License. You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
# WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
# License for the specific language governing permissions and limitations under
# the License.


"""
"""

import ast
import logging

logger = logging.getLogger('eos_install_config')

DOCUMENTATION = '''
'''

EXAMPLES = '''
'''

def compile_address_book(host_file):
    fact = {'address_book': dict()}

    re_group = re.compile('\[(?P<group>.*)\]|(?P<entry>(\w+.*)+)')

    with open('../{}'.format(host_file), 'r') as f:
        for line in f.readlines():
            match = re.match(re_group, line)

            if match is not None:
                if match.group('group') is not None:
                    group = match.group('group')
                    fact['address_book'][group] = list()
                else:
                    fqdn = '{}.service'.format(match.group('entry'))
                    entry = (
                        match.group('entry'),
                        socket.gethostbyname_ex(fqdn)[2][0]
                    )
                    fact['address_book'][group].append(entry)

    return fact

def verify_policies(address_book, policies):
    groups = address_book.keys()

    for p in policies:
        if p[0] not in groups:
            raise Exception('We found a policy without hosts. Please, clean the legacy policy. The group is {} and the policy is {}'.format(p[0], p))
        elif p[1] not in groups:
            raise Exception('We found a policy without hosts. Please, clean the legacy policy. The group is {} and the policy is {}'.format(p[1], p))

def main():
    module = AnsibleModule(
        argument_spec=dict(
            host_file=dict(required=True),
            policies=dict(required=True),
        ),
        supports_check_mode=True
    )

    host_file = module.params['host_file']
    policies = ast.literal_eval(module.params['policies'])

    fact = compile_address_book(host_file)
    verify_policies(fact['address_book'], policies)

    module.exit_json(ansible_facts=fact)

from ansible.module_utils.basic import *
import re
import socket

main()
