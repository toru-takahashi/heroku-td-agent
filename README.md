# Treasure Agent on Heroku

This repository contains everything to run Treasure Agent with Deskcom plugin on Heroku.

## Setup

There is no step 1. Just click on the Heroku Deploy button here.

<a href="https://heroku.com/deploy?template=https://github.com/toru-takahashi/heroku-td-agent"><img src="https://www.herokucdn.com/deploy/button.png"/></a>

By default, a new Treasure Data account is created.
If you already have an account with a [Treasure Data API key](http://docs.treasuredata.com/articles/get-apikey), fill out the "TREASURE\_DATA\_API\_KEY\_OVERRIDE" field.

And, you needs the following parameters.

- "DESKCOM\_SUBDOMAIN"
- "DESKCOM\_CONSUMER\_KEY"
- "DESKCOM\_CONSUMER\_SECRET\_KEY"
- "DESKCOM\_OAUTH\_TOKEN"
- "DESKCOM\_OAUTH\_TOKEN\_SECRET" 

<img src="http://gyazo.com/ea06dd57a6bee2fa882c7902c26331c2.png"/></a>


By default, td-agent.conf is following settings.

```ruby
####
## Input descriptions:
##
<source>
  type deskcom
  tag td.deskcom.cases
  subdomain "#{ENV['DESKCOM_SUBDOMAIN']}"
  consumer_key "#{ENV['DESKCOM_CONSUMER_KEY']}"
  consumer_secret "#{ENV['DESKCOM_CONSUMER_SECRET_KEY']}"
  oauth_token "#{ENV['DESKCOM_OAUTH_TOKEN']}"
  oauth_token_secret "#{ENV['DESKCOM_OAUTH_TOKEN_SECRET']}"
  store_file cases.yml
  output_format simple
  input_api cases
  time_column updated_at
</source>
<source>
  type deskcom
  tag td.deskcom.replies
  subdomain "#{ENV['DESKCOM_SUBDOMAIN']}"
  consumer_key "#{ENV['DESKCOM_CONSUMER_KEY']}"
  consumer_secret "#{ENV['DESKCOM_CONSUMER_SECRET_KEY']}"
  oauth_token "#{ENV['DESKCOM_OAUTH_TOKEN']}"
  oauth_token_secret "#{ENV['DESKCOM_OAUTH_TOKEN_SECRET']}"
  store_file replies.yml
  output_format simple
  input_api replies
  time_column created_at
</source>
####
## Output descriptions:
##
# Treasure Data output
# match events whose tag is td.DATABASE.TABLE
<match td.*.*>
  type tdlog
  apikey "#{ENV['TREASURE_DATA_API_KEY_OVERRIDE'] || ENV['TREASURE_DATA_API_KEY']}"
  use_ssl true
  auto_create_table
  flush_at_shutdown true
  # Memory Buffer
  # buffer_type memory
  # File Buffer (2GB = 8m * 256)
  buffer_type file
  buffer_path ./buffer/td
  buffer_chunk_limit 8m
  buffer_queue_limit 512
  </match>
  ## match tag=debug.** and dump to console
<match debug.**>
  type stdout
</match>
```
