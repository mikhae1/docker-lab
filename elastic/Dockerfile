FROM docker.elastic.co/elasticsearch/elasticsearch:5.3.1

COPY config/ config/

ENV ELK_VERSION=5.3.1

# install plugins
RUN elasticsearch-plugin install http://dl.bintray.com/content/imotov/elasticsearch-plugins \
/org/elasticsearch/elasticsearch-analysis-morphology/$ELK_VERSION \
/elasticsearch-analysis-morphology-$ELK_VERSION.zip \
  && elasticsearch-plugin install https://artifacts.elastic.co/downloads/elasticsearch-plugins \
/analysis-ukrainian/analysis-ukrainian-$ELK_VERSION.zip
