# tci-elk-logging

This is a small contribution aimed at getting you started more quickly when about to dump TCI log files to ELK (Elasticsearch Logstash, and Kibana)

You'll find the two necessary files:
<ul>
  <li><code>tci-log.conf</code>: the configuration file for logstash,</li>
  <li><code>patterns/tci-grok-patterns</code>: the patterns file for logstash's <code>grok</code> filter.
</ul>

This extracts a number of properties from the logs, which will be dropped in Elasticsearch, that you can use in Kibana to discover you data and visualise it:
<ul>
  <li>errlvl</li>
  <li>tcipt</li>
</ul>
as well as the following ones for BusinessWorks applications:
<ul>
  <li><code>tcijobid</code> the job ID</li>
  <li><code>tcipiid</code> the process ID</li>
  <li><code>tcippiid</code> the parent process ID (if this is about a subprocess)</li>
  <li><code>tciactivity</code> the activity name</li>
  <li><code>tciprocess</code> the process name</li>
  <li><code>tcimod</code> the module name</li>
  <li><code>tciapp</code> the application name</li>
</ul>
 
There are many options when it comes to running the ELK stack you'll dump those contents to, included running it directly from GCP (Hosted on GCP, offered by Elasticsearch) or AWS (Amazon Elasticsearch Service).
I tried Bitnami's ELK image for Amazon EC2 and could recommend it: it is well documented, leaves access to server via <code>ssh</code>, and leaves a lot of flexibility when it comes to configuration, including installing TCI's <code>tibcli</code>.
You get all the documentation on <a href="https://docs.bitnami.com/aws/apps/elk/">Bitnami's website</a>. 

Should you happen to use Bitnami's, here is what to do with the two files:
<ul>
  <li><code>cp tci-log.conf /opt/bitnami/logstash/conf</code></li>
  <li><code>mkdir /opt/bitnami/logstash/patterns</code></li>
  <li><code>cp tci-grok-patterns /opt/bitnami/logstash/patterns</code></li>
</ul>

And here is how you would start logstash (making sure you are logged in TCI with <code>tibcli</code>:
<code>./tibcli monitor applog -s <your application's name> | sudo /opt/bitnami/logstash/bin/logstash -f /opt/bitnami/logstash/conf/tci.conf</code>

