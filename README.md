# Python-Slack-Discovery-SDK

👋🏼 Welcome to the Python-Slack-Discovery-SDK! This project aims to make using the [Slack Discovery API's](https://api.slack.com/enterprise/discovery/methods#methods) easier.

> 🚨 Note: This SDK is only accessible to customer developers with access to the Discovery API (Enterprise accounts) or partners who have been onboarded to the Security and Compliance partner program. To learn more about the Discovery APIs, please [visit our help center](https://slack.com/help/articles/360002079527-A-guide-to-Slacks-Discovery-APIs). 🚨


# Using the SDK

Below are the steps needed to use the Discovery SDK.


## Check Your Python Version

Ensure you have Python 3.6 or greater by checking your Python version:

```bash
python3 --version
Python 3.9.5
```

## Set your Virtual Environment (Optional, but Recommended)

```bash
# Setup your virtual environment
python3 -m venv .venv
source .venv/bin/activate
```

## Install Dependencies and required packages:
```bash
pip install -e .
```
## Set Environmental Variables

Set your enterprise level token, which should have `discovery:read` and `discovery:write` scopes:

```bash
export SLACK_DISCOVERY_SDK_TEST_ENTERPRISE_TOKEN='xoxp-2243093387093-2239369144111....' 
```
> **Note:** if you want to use all of the functionality that the SDK has to offer, you will need to export a few more environmental variables: 

```bash

# 1. A normal bot token with chat:write, channels:read
export SLACK_DISCOVERY_SDK_TEST_BOT_TOKEN=xoxb-xxxx
# 2. A test workspace ID in the Enterprise Org
#    SLACK_DISCOVERY_SDK_TEST_BOT_TOKEN should have the access to this workspace
export SLACK_DISCOVERY_SDK_TEST_TEAM_ID=T1234567890
# 3. A test channel ID in the Enterprise Org
#    SLACK_DISCOVERY_SDK_TEST_BOT_TOKEN should have the access to this channel
export SLACK_DISCOVERY_SDK_TEST_CHANNEL_ID=C1234567890
```

## Running the Examples
Use the following command to run a script which calls the `discovery.enterprise.info` endpoint. This endpoint returns basic information about the Enterprise Grid org where the app is installed, including all workspaces (teams). 

```python
python3 slack_discovery_sdk/examples/get_enterprise_info.py
```

You should see a response similar to the following (note the result below has been truncated for readability):

```python
DEBUG:slack_discovery_sdk.base_client:Rate limit metrics: {'last_second_requests': 0, 'last_minute_requests_per_api_method': {}, 'successful_call_counts': {}, 'failed_call_counts': {}}...
{'ok': True, 'enterprise': {'id': 'T027****D2R', 'name': 'Enterprise-******-Sandbox', 'domain': 'test-**', 'email_domain': '', 'icon': {'image_102': 'https://a.slack-edge.com/80588/img/avatars-teams/ava_****2.png', 'image_default': True}, 'is_verified': False, 'teams': [{'id': 'T027****D2R', 'name': 'Enterprise-****-Sandbox', 'domain': 'test-****', 'email_domain': '', 'icon': {'image_102': 'https://a.slack-edge.com/80588/img/avatars-teams/****-102.png', 'image_default': True}, 'is_verified': False, 'enterprise_id': '****', 'is_enterprise': 0, 'created': 1625594757, 'archived': False, 'deleted': False, 'discoverable': 'unlisted'}]}}
```

There are various other scripts in the `slack_discovery_sdk/examples` folder, with each script serving to solve a different use case. Below you can find some basic information about each script:

💳 <b>`DLP_call_pattern.py`</b> 💳
* This script involves using the tombstoning capabilities of the Discovery SDK to check for messages that contain sensitive information. If sensitive information is detected by our script (for example a credit card number), the message is tombstoned, and the user is notified that their message is being reviewed.
* Once you run this script, you should see that one of your 
messages in the channel which you set in your env variable (SLACK_DISCOVERY_SDK_TEST_CHANNEL_ID) should have been tombstoned. The message should now say `This message is being scanned to make sure it complies with your team’s data security policies.`

🙋🏾‍♀️ <b>`user_based_eDiscovery_pattern.py`</b> 👩🏻‍🏫
* This script retrieves all of the conversations (channels) and messages a particular user is in. It then outputs those 
conversations to a file, and stores them in the following format: `YYYY/MM/DD/user_id/channel_id/discovery_conversations_history.json`. 

👩🏻‍🏫 <b>`audit_logs_pattern.py`</b> 👩🏻‍🏫
* This script will use the [Audit Logs API](https://api.slack.com/admins/audit-logs) to find all of the
channels that a particular user has created. As is the 
case with the `user_based_eDiscovery` script, it will only
be useful if you have a paricular user which you want to see details about. This script will output the channel creation events associated with a particular user_id to in the following format: `YYYY/MM/DD/user_id/audit_logs/public_channel_created.json`. 

## Considerations
The SDK and examples are to aid in your development process. Please feel free to use this as a learning exercise, and to build on top of these examples, but the examples shown above are by no means a complete solution. 

## Running tests

```bash
# Setup your virtual environment
python --version  # make sure if you're using Python 3.6+
python -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install -e ".[testing]"

# Set required env variables
# 1. An admin user token with discovery:read, discovery:write
export SLACK_DISCOVERY_SDK_TEST_ENTERPRISE_TOKEN=xoxp-xxx
# 2. A normal bot token with chat:write, channels:read
export SLACK_DISCOVERY_SDK_TEST_BOT_TOKEN=xoxb-xxxx
# 3. A test workspace ID in the Enterprise Org
#    SLACK_DISCOVERY_SDK_TEST_BOT_TOKEN should have the access to this workspace
export SLACK_DISCOVERY_SDK_TEST_TEAM_ID=T1234567890
# 4. A test channel ID in the Enterprise Org
#    SLACK_DISCOVERY_SDK_TEST_BOT_TOKEN should have the access to this channel
export SLACK_DISCOVERY_SDK_TEST_CHANNEL_ID=C1234567890
pytest tests/

# You can check logs/pytest.log for trouble shooting
```