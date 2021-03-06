CMPJSON = 2>>log.txt | ./compare_json --ignore-log -
CMPTXT = 2>>log.txt | diff -b -
#CMPTXT = 2>>log.txt >

JSON_DIR = ../jsoncpp-src-0.5.0-patched

JSON_SRCS = \
	$(JSON_DIR)/src/lib_json/json_value.cpp \
	$(JSON_DIR)/src/lib_json/json_reader.cpp \
	$(JSON_DIR)/src/lib_json/json_writer.cpp

CPPFLAGS = -O3 -Wall -g -I.. -I../jsoncpp-src-0.5.0-patched/include

BINARIES = \
	compare_json \
	TestBinaryIO \
	TestChannel \
	TestDataSample \
	TestFilesystemKVS \
	TestJson \
	TestRange \
	TestTile \
	TestTileIndex

ALL = \
	$(BINARIES) \
	test-annebug \
	test-multi-gettile \
	test-multi-gettile-multi-uid \
	test-import-bt \
	test-import-json-single \
	test-import-json-multiple \
        test-export-csv \
        test-export-csv-multiple-uid \
	test-import-json-format \
	test-import-json-single-entry \
        test-import-null \
        test-import-false \
        test-import-false-atop-empty \
        test-import-true

all: $(ALL)

clean:
	rm -rf $(BINARIES) log.txt

VPATH = ..

%.test: %
	./$^

compare_json: compare_json.cpp $(JSON_SRCS)
	g++ $(CPPFLAGS) -o $@ $^ $(LDFLAGS)

TestRange: TestRange.cpp utils.cpp
	g++ $(CPPFLAGS) -o $@ $^ $(LDFLAGS)
	./$@

TestJson: TestJson.cpp utils.cpp $(JSON_SRCS)
	g++ $(CPPFLAGS) -o $@ $^ $(LDFLAGS)
	./$@

TestBinaryIO: TestBinaryIO.cpp BinaryIO.cpp utils.cpp
	g++ $(CPPFLAGS) -o $@ $^ $(LDFLAGS)
	./$@

TestDataSample: TestDataSample.cpp utils.cpp
	g++ $(CPPFLAGS) -o $@ $^ $(LDFLAGS)
	./$@

TestTileIndex: TestTileIndex.cpp utils.cpp $(JSON_SRCS)
	g++ $(CPPFLAGS) -Wall -g -I .. -I /opt/local/include -o $@ $^ $(LDFLAGS)
	./$@

TestTile: TestTile.cpp BinaryIO.cpp Log.cpp Tile.cpp utils.cpp $(JSON_SRCS)
	g++ $(CPPFLAGS) -Wall -g -I .. -I /opt/local/include -o $@ $^ $(LDFLAGS)
	./$@

TestChannel: TestChannel.cpp BinaryIO.cpp Channel.cpp FilesystemKVS.cpp KVS.cpp Log.cpp Tile.cpp utils.cpp $(JSON_SRCS)
	g++ $(CPPFLAGS) -Wall -g -I .. -I /opt/local/include -o $@ $^ $(LDFLAGS)
	./$@

TestFilesystemKVS: TestFilesystemKVS.cpp FilesystemKVS.cpp KVS.cpp utils.cpp Log.cpp $(JSON_SRCS)
	g++ $(CPPFLAGS) -Wall -g -I .. -I /opt/local/include -o $@ $^ $(LDFLAGS)
	./$@

test-annebug: compare_json
	rm -rf anne.kvs
	mkdir anne.kvs
	../import anne.kvs 1 A_Cheststrap testdata/anne-cheststrap-bug/4e386a43.bt $(CMPJSON) output/test-annebug-1
	../gettile anne.kvs 1 A_Cheststrap.Respiration 0 2563125 $(CMPJSON) output/test-annebug-2
	../import anne.kvs 1 A_Cheststrap testdata/anne-cheststrap-bug/345.bt $(CMPJSON) output/test-annebug-3
	../gettile anne.kvs 1 A_Cheststrap.Respiration 0 2563125  $(CMPJSON) output/test-annebug-4

test-multi-gettile: compare_json
	rm -rf anne.kvs
	mkdir anne.kvs
	../import anne.kvs 1 A_Cheststrap testdata/anne-cheststrap-bug/4e386a43.bt $(CMPJSON) output/test-multi-gettile-1
	../gettile anne.kvs 1 --multi A_Cheststrap.Respiration,A_Cheststrap.EKG 0 2563125 $(CMPJSON) output/test-multi-gettile-2

test-multi-gettile-multi-uid: compare_json
	rm -rf anne.kvs
	mkdir anne.kvs
	../import anne.kvs 1 A_Cheststrap testdata/anne-cheststrap-bug/4e386a43.bt $(CMPJSON) output/test-multi-gettile-multi-uid-1
	../import anne.kvs 2 A_Cheststrap2 testdata/anne-cheststrap-bug/4e386a43.bt $(CMPJSON) output/test-multi-gettile-multi-uid-2
	../gettile anne.kvs --multi 1.A_Cheststrap.Respiration,1.A_Cheststrap.EKG,2.A_Cheststrap2.Respiration 0 2563125 $(CMPJSON) output/test-multi-gettile-multi-uid-3

test-import-bt: compare_json
	rm -rf foo.kvs
	mkdir -p foo.kvs
	../import foo.kvs 1 ar-basestation testdata/ar_basestation/4cfcff7d.bt $(CMPJSON) output/test-import-bt-1
	../export foo.kvs 1 ar-basestation.Humidity                            $(CMPTXT) output/test-import-bt-2
	../export foo.kvs 1 ar-basestation.Microphone                          $(CMPTXT) output/test-import-bt-3
	../export foo.kvs 1 ar-basestation.Pressure                            $(CMPTXT) output/test-import-bt-4
	../export foo.kvs 1 ar-basestation.Temperature                         $(CMPTXT) output/test-import-bt-5

test-import-json-single: compare_json
	rm -rf foo.kvs
	mkdir -p foo.kvs
	../import foo.kvs 1 rphone testdata/json_import20110805-4317-1pzfs94-0.json $(CMPJSON) output/test-import-json-single-1
	../export foo.kvs 1 rphone.accuracy                                         $(CMPTXT) output/test-import-json-single-2
	../export foo.kvs 1 rphone.altitude                                         $(CMPTXT) output/test-import-json-single-3
	../export foo.kvs 1 rphone.bearing                                          $(CMPTXT) output/test-import-json-single-4
	../export foo.kvs 1 rphone.latitude                                         $(CMPTXT) output/test-import-json-single-5
	../export foo.kvs 1 rphone.longitude                                        $(CMPTXT) output/test-import-json-single-6
	../export foo.kvs 1 rphone.speed                                            $(CMPTXT) output/test-import-json-single-7
	../export foo.kvs 1 rphone.provider		                            $(CMPTXT) output/test-import-json-single-8

test-import-json-multiple: compare_json
	rm -rf foo.kvs
	mkdir -p foo.kvs
	../import foo.kvs 1 rphone testdata/multiple.json                $(CMPJSON) output/test-import-json-multiple-1
	../export foo.kvs 1 rphone.latitude rphone.altitude rphone.speed $(CMPTXT) output/test-import-json-multiple-2

test-export-csv:
	rm -rf foo.kvs
	mkdir -p foo.kvs
	../import foo.kvs 1 rphone testdata/multiple.json                      $(CMPJSON) output/test-export-csv-1
	../export --csv foo.kvs 1 rphone.latitude rphone.altitude rphone.speed $(CMPTXT) output/test-export-csv-2

test-export-csv-multiple-uid:
	rm -rf foo.kvs
	mkdir -p foo.kvs
	../import foo.kvs 1 rphone testdata/multiple.json                           $(CMPJSON) output/test-export-csv-multiple-uid-1
	../import foo.kvs 2 rphone2 testdata/multiple.json                          $(CMPJSON) output/test-export-csv-multiple-uid-2
	../export --csv foo.kvs 1.rphone.latitude 1.rphone.speed 2.rphone2.altitude $(CMPTXT) output/test-export-csv-multiple-uid-3

test-import-json-format: compare_json
	rm -rf foo.kvs
	mkdir -p foo.kvs
	../import foo.kvs 1 rphone --format json testdata/json_import20110805-4317-1pzfs94-0                       $(CMPJSON) output/test-import-json-format-1
	../export foo.kvs 1 rphone.latitude rphone.altitude rphone.speed                                           $(CMPTXT) output/test-import-json-format-2
	../export foo.kvs --csv 1 rphone.latitude rphone.altitude rphone.speed                                     $(CMPTXT) output/test-import-json-format-3
	../export foo.kvs --csv --start 1312585229 --end 1312585232 1 rphone.latitude rphone.altitude rphone.speed $(CMPTXT) output/test-import-json-format-3
	../export foo.kvs --csv --start 1312585230 --end 1312585232 1 rphone.latitude rphone.altitude rphone.speed $(CMPTXT) output/test-import-json-format-4
	../export foo.kvs --csv --start 1312585230 --end 1312585231 1 rphone.latitude rphone.altitude rphone.speed $(CMPTXT) output/test-import-json-format-5
	../export foo.kvs --csv --start 1312585240 1 rphone.latitude rphone.altitude rphone.speed                  $(CMPTXT) output/test-import-json-format-6

test-import-json-single-entry: compare_json
	rm -rf foo.kvs
	mkdir -p foo.kvs
	../import foo.kvs 1 myiphone testdata/mymee-anne-import-test-110901.json $(CMPJSON) output/test-import-json-single-entry-1
	../export foo.kvs 1 myiphone.Dopaboost                                   $(CMPTXT) output/test-import-json-single-entry-2
	../export foo.kvs 1 myiphone.Dopaboost._comment                          $(CMPTXT) output/test-import-json-single-entry-3
	../import foo.kvs 1 myiphone testdata/mymee-anne-import-test-110901.json $(CMPJSON) output/test-import-json-single-entry-4
	../export foo.kvs 1 myiphone.Dopaboost                                   $(CMPTXT) output/test-import-json-single-entry-2
	../export foo.kvs 1 myiphone.Dopaboost._comment                          $(CMPTXT) output/test-import-json-single-entry-3
	../export foo.kvs --csv 1 myiphone.Dopaboost myiphone.Dopaboost._comment $(CMPTXT) output/test-import-json-single-entry-5

test-import-null: compare_json
	rm -rf foo.kvs
	mkdir -p foo.kvs
	../import foo.kvs 1 testdevice testdata/null.json $(CMPJSON) output/test-import-null-1
	../export foo.kvs 1 testdevice.latitude testdevice.longitude testdevice.accuracy $(CMPTXT) output/test-import-null-2

test-import-false: compare_json
	rm -rf foo.kvs
	mkdir -p foo.kvs
	../import foo.kvs 1 testdevice testdata/false1.json $(CMPJSON) output/test-import-false-1
	../export foo.kvs 1 testdevice.latitude testdevice.longitude testdevice.accuracy $(CMPTXT) output/test-import-false-2
	../import foo.kvs 1 testdevice testdata/false2.json $(CMPJSON) output/test-import-false-3
	../export foo.kvs 1 testdevice.latitude testdevice.longitude testdevice.accuracy $(CMPTXT) output/test-import-false-4

test-import-false-atop-empty: compare_json
	rm -rf foo.kvs
	mkdir -p foo.kvs
	../import foo.kvs 1 testdevice testdata/false2.json $(CMPJSON) output/test-import-false-atop-empty-1
	../export foo.kvs 1 testdevice.latitude testdevice.longitude testdevice.accuracy $(CMPTXT) output/test-import-false-atop-empty-2

# Importing 'true' should give an error message
test-import-true: compare_json
	rm -rf foo.kvs
	mkdir -p foo.kvs
	(../import foo.kvs 1 testdevice testdata/true.json; exit 0) $(CMPJSON) output/test-import-true
