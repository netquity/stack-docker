# https://github.com/elastic/elasticsearch-docker

FROM docker.elastic.co/elasticsearch/elasticsearch-oss:6.2.2

ADD config/sg/ /usr/share/elasticsearch/config/sg/
ADD bin/ /usr/share/elasticsearch/bin/

COPY --chown=1000:0 bin/docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh
COPY --chown=1000:0 bin/setup-searchguard.sh /usr/local/bin/setup-searchguard.sh

# Openshift overrides USER and uses ones with randomly uid>1024 and gid=0
# Allow ENTRYPOINT (and ES) to run even with a different user
RUN chgrp 0 /usr/local/bin/docker-entrypoint.sh && \
    chmod 0775 /usr/local/bin/docker-entrypoint.sh

RUN chgrp 0 /usr/local/bin/setup-searchguard.sh && \
    chmod 0775 /usr/local/bin/setup-searchguard.sh

# Search Guard plugin
# https://github.com/floragunncom/search-guard/wiki
RUN elasticsearch-plugin install --batch com.floragunn:search-guard-6:6.2.2-22.0 \
	&& chmod +x plugins/search-guard-6/tools/*.sh \
	&& chown -R elasticsearch config/sg/ \
	&& chmod -R go= config/sg/

# Add your elasticsearch plugins setup here
# Example: RUN elasticsearch-plugin install analysis-icu
