TMP_DIR=./tmp

TMP_VER=$(TMP_DIR)/version_num.tmp

common_CFLAGS=@PROFILER_FLAGS@ -I@MEMCACHED_DIR@/include
common_LDFLAGS=-g @PROFILER_LDFLAGS@ @MEMCACHED_DIR@/libmcd_util.la

lib_LTLIBRARIES = bucket_engine.la
bucket_engine_la_SOURCES= bucket_engine.c bucket_engine.h
bucket_engine_la_CFLAGS = $(common_CFLAGS)
bucket_engine_la_LDFLAGS= -module -dynamic $(common_LDFLAGS)

noinst_LTLIBRARIES = mock_engine.la
mock_engine_la_SOURCES = mock_engine.c
mock_engine_la_CFLAGS = $(common_CFLAGS)
mock_engine_la_LDFLAGS = -module -dynamic -rpath /nowhere $(common_LDFLAGS)

noinst_PROGRAMS = testapp
testapp_SOURCES = testapp.c
testapp_DEPENDENCIES = mock_engine.la
testapp_CFLAGS = $(common_CFLAGS)
testapp_LDFLAGS = $(common_LDFLAGS)

pkgdata_SCRIPTS = management/create_bucket.py  management/delete_bucket.py  management/list_buckets.py  management/mc_bin_client.py  management/memcacheConstants.py management/collectd.py management/collectd_memcached_buckets.py management/types.db

EXTRA_DIST = $(pkgdata_SCRIPTS)

test: all testapp
	./testapp

version:
	test -d $(TMP_DIR) || mkdir $(TMP_DIR)
	git describe | sed s/-/_/g > $(TMP_VER)

bdist: version
	rm -f ./bucket_engine_*.tar.gz
	rm -rf $(TMP_DIR)/bucket_engine
	mkdir $(TMP_DIR)/bucket_engine
	cp .libs/bucket_engine.so $(TMP_DIR)/bucket_engine
	(cd $(TMP_DIR); tar cf - bucket_engine) | gzip -9 > bucket_engine_`cat $(TMP_VER)`-`uname -s`.`uname -m`.tar.gz
	echo created bucket_engine_`cat $(TMP_VER)`-`uname -s`.`uname -m`.tar.gz
