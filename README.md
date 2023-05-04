# Examples 

1. TOPICS BLUEPRINT:
    ```
    #Static Vaule
    version: v1 
    #-------------------------------------------------------
    #Global Configuration (Applies to all topics as default)
    retention_period: #Defines how many time the messages will be retained
      weeks: 0    #Number of weeks
      days: 0     #Number of days
      minutes: 0  #Number of minutes
      seconds: 3  #Number of seconds
    max_message_bytes: 1048576  #Maximum size per message
    number_of_partitions: 1     #Number of partitions per topic
    delete_policy: delete       #Deletion policy (delete or compact)
    #-------------------------------------------------------
    #Topic Definition
    topics:
      - &topic1 domain.topicname:  # Topic 1 Name
        name: *topic1                         # Relation between topic and name
        max_message_bytes: 1048579            # Overrides the maximum size per message defined at Global Configuration
      - &topic2 domain.topicname2:
        name: *topic2                         
        max_message_bytes: 1048573
      - &topic3 domain.topicname3:
        name: *topic3                         
        max_message_bytes: 1048571
        delete_policy: compact
    #-------------------------------------------------------
    #Service accounts and acl's definition
    service_accounts:
      - sa:
        name: serviceaccountname    #Service account name
        acls:
          - topic: *topic1    # Acl definition for topic 1
            read: false       # Deny read permisions for this service account and topic1
            write: false       # Allow write permisions for this service account and topic1
            idempotent-write: false
          - topic: *topic3
            read: false
            write: false
            idempotent-write: false
    ```

2. STREAMS Example:
    ```
    #Static Vaule
    version: v1 
    #-------------------------------------------------------
    #Stream Applications Definition
    streams:
      - &streamapp1 domain.streamappname:   # Stream application name
        name: *streamapp1                      # Mandatory - Relation between stream and name
      - &streamapp2 domain.streamappname2:
        name: *streamapp2
    #-------------------------------------------------------
    #Service accounts and acl's definition
    service_accounts:
      - sa:
        name: serviceaccountname     #Service account name
        acls:
          - app: *streamapp1   # Stream definition for stream application 1
            read: false         # Allow read permisions for this service account and stream application 1
            write: false        # Allow write permisions for this service account and stream application 1
          - app: *streamapp2
            read: false
            write: false
    ```
