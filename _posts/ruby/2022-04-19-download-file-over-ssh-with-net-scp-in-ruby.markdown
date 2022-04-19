---
layout: post
title:  "Download A File Over SSH With Net::SCP In Ruby"
author: Denis Kobare
date:   2022-04-19 10:30:00 +0300
img: /assets/img/svg/ruby.svg
categories: programming
sub_category: ruby
type: tutorials
technology: Ruby
permalink: "category/:categories/ruby/tutorials/:year:month/:title"
---

### Definition

Downloading files from a remote server is part of the frequent tasks for a database admin. A DB admin would write scripts to dump backup files and download them to a backup storage.

While this document only demonstrates a basic implementation of NET::SCP to download a file over SSH, the concept can be used to build useful tools (with decent user interfaces) that automate backup processes.

  
<br>
### The Ruby File

{% highlight ruby %}

require "net/scp"

def downloadFileOverSCP(host, port, user, remoteFile, localFile, sshKey, recursive = false)

  options = {
      :ssh => {
          :keys => sshKey,
          :port => port
      },
      :recursive => recursive
  }

  Net::SCP.download!(host, user, remoteFile, localFile, options) do |ch, name, received, total|

    percentage = format('%.2f', received.to_f / total.to_f * 100) + '%'

    print "Saving to #{name}: Received #{received} of #{total} bytes" + " (#{percentage})\r"

    STDOUT.flush
  end
  puts "Download completed! "
end

# e.g

downloadFileOverSCP(
    '41.35.26.17',
    22,
    'ubuntu',
    '/home/ubuntu/databases/backup_data.sql',
    '/home/techietuts/Databases/backup_data.sql', # You can specify a different file name
    '/home/techietuts/key_pair.pem'
)

{% endhighlight %} 



<br>
*Thanks for reading, see you in the next one!*
