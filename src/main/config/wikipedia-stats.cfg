job {
  name: "wikipedia-stats"
  factory_class: "org.apache.samza.job.yarn.YarnJobFactory"

  # YARN config
  yarn {
    package_path: "file://${basedir}/target/${project.artifactId}-${pom.version}-dist.tar.gz"
  }

  # Task config
  task {
    class: "samza.examples.wikipedia.task.WikipediaStatsStreamTask"
    input: "kafka.wikipedia-edits"
    window_ms: "10000"
    checkpoint {
      factory: "org.apache.samza.checkpoint.kafka.KafkaCheckpointManagerFactory"
      system: "kafka"
      # Normally, this would be 3, but we have only one broker.
      replication_factor: 1
    }
  }

  # Serializers
  serializer {
    name: "json"
    class: "org.apache.samza.serializers.JsonSerdeFactory"
  }
  serializer {
    name: "string"
    class: "org.apache.samza.serializers.StringSerdeFactory"
  }
  serializer {
    name: "integer"
    class: "org.apache.samza.serializers.IntegerSerdeFactory"
  }


  # Systems
  system {
    name: "kafka"
    samza_factory: "org.apache.samza.system.kafka.KafkaSystemFactory"
    samza_msg_serde: "json"
    consumer {
      zookeeper_connect: "localhost:2181/"
      auto_offset_reset: "largest"
    }
    producer {
      metadata_broker_list: "localhost:9092"
      producer_type: "sync"
      # Normally, we'd set this much higher, but we want things to look snappy in the demo.
      batch_num_messages: 1
    }
  }

  # Key-value storage
  store {
    name: "wikipedia-stats"
    factory: "org.apache.samza.storage.kv.RocksDbKeyValueStorageEngineFactory"
    changelog: "kafka.wikipedia-stats-changelog"
    key_serde: "string"
    msg_serde: "integer"

    # Normally, we'd set this much higher, but we want things to look snappy in the demo.
    write_batch_size: 0
    object_cache_size: 0
  }
}
