How to use
==========

Input arguments
---------------

items_to_score (Dict)
~~~~~~~~~~~~~~~~~~~~~
Dictionary of modules to score and their respective domains and information specifying where the necessary responses can be found in the data.
.. code-block:: python
Example:
{'items_to_score': {
                    'C30': {
                           'domains_to_score': ['QL2', 'PF2'],
                           'variable_info': {
                                            'question_1': 'Q1',
                                            'question_2': 'Q2',
                                            'question_3': 'Q3',
                                            'question_4': 'Q4',
                                            'question_5': 'Q5',
                                            'question_6': 'Q6',
                                            'question_7': 'Q7',
                                            'question_29': 'Q29',
                                            'question_30': 'Q30'
                                            }
                           }
                    }
}
..

variables_to_stratify (Dict, optional)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Dictionary of variables to stratify. Defaults to None.
Example:
.. code-block:: python
{'variables_to_stratify':
    {
        'Age': {
            'end': 39,
            'datatype': 'int'
                }
    }
}
..


organisation_ids (list[int], optional):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
List of organisation IDs to include.
Defaults to None - therewith including all organisations.

Python client example
---------------------

To understand the information below, you should be familiar with the vantage6
framework. If you are not, please read the `documentation <https://docs.vantage6.ai>`_
first, especially the part about the
`Python client <https://docs.vantage6.ai/en/main/user/pyclient.html>`_.

.. code-block:: python

  from vantage6.client import Client

  server = 'http://localhost'
  port = 7601
  api_path = '/api'
  private_key = None
  username = 'dev_admin'
  password = 'password'
  collaboration_id = 1
  organization_ids = [2]

  # Create connection with the vantage6 server
  client = Client(server, port, api_path)
  client.setup_encryption(private_key)
  client.authenticate(username, password)

  input_ = {
    'method': 'central',
    'kwargs': {
        'items_to_score': {'items_to_score': {
                                            'C30': {
                                                'domains_to_score': ['QL2', 'PF2'],
                                                'variable_info': {
                                                    'question_1': 'surv1',
                                                    'question_2': 'surv2',
                                                    'question_3': 'surv3',
                                                    'question_4': 'surv4',
                                                    'question_5': 'surv5',
                                                    'question_6': 'surv70',
                                                    'question_7': 'surv71',
                                                    'question_29': 'surv120',
                                                    'question_30': 'surv121'
                                                }
                                            }}}',
        'variables_to_stratify': {'Age': {'start': 40}, datatype: 'int'},
        'organization_ids': [1, 2, 4]
    }
  }

  my_task = client.task.create(
      collaboration=collaboration_id,
      organizations=organization_ids,
      name='EORTC QLQ scoring',
      description='Vantage6 algorithm that performs EORTC QLQ function and symptom scale scoring.',
      image='v6-eortc-qlq-scoring',
      input_=input_,
      databases=[{'label': 'default'}],
  )

  task_id = my_task.get('id')
  results = client.wait_for_results(task_id)

User Interface (UI) example
---------------------------
For users:
~~~~~~~~~~
When using the algorithm in the Vantage6 UI, you can select the algorithm from the list of available algorithms, given it is submitted and approved for your algorithm store
This will allow you to specify the EORTC QLQ module, and the domains of interest.
It is also necessary to specify the variable information, if it is not available by default. This can be done by selecting the corresponding columns from the and listing them next to the corresponding question.


For algorithm store managers and reviewers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
If using a common terminology in your collaboration, consider including the variable information in the default algorithm input that can be defined when submitting an algorithm to the algorithm store.