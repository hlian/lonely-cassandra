FROM cassandra:3.9

ENV DEBIAN_FRONTEND noninteractive
ENV CONFIG /etc/cassandra/cassandra.yaml

# Run base config script
RUN sed -ie "s/num_tokens/\#num_tokens/" "$CONFIG"
# Be your own seed
RUN sed -ie "s/- seeds: \"127.0.0.1\"/- seeds: \"$SEEDS\"/" "$CONFIG"
# With virtual nodes disabled, we need to manually specify the token
RUN echo "JVM_OPTS=\"\$JVM_OPTS -Dcassandra.initial_token=0\"" >> $CASSANDRA_CONFIG/cassandra-env.sh
# Pointless in one-node cluster, saves about 5 sec waiting time
RUN echo "JVM_OPTS=\"\$JVM_OPTS -Dcassandra.skip_wait_for_gossip_to_settle=0\"" >> $CASSANDRA_CONFIG/cassandra-env.sh

RUN cat "$CONFIG"

EXPOSE 9042

CMD ["/docker-entrypoint.sh", "cassandra", "-f"]
