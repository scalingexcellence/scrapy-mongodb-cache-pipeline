Description
===========
A base pipeline and a decorator to allow you to cache item fields in MongoDB collections.

Install
=======
   pip install "ScrapyMongoDBCache"

   Make your pipeline derive from MongoCacheBasePipeline and decorate the process_item method with the @cache decorator. The parameters for the decorator are: 1st the database and the collection to use in MongoDB, 2nd item's field to be used as a key and 3rd, array of fields to be used as cached values. Calls to process_item() aren't made if the key exists in cache. The cache assumes that MongoDB is installed in localhost. If it's installed somewhere else, then set the mongohost spider argument when you run scrapy (e.g. scrapy crawl myspider -a mongohost=username:password@host1:port1). Example implementation:
----------------------------

    from geopy import geocoders
    from mongocache import *

    class GeocodingPipeline(MongoCacheBasePipeline):
        def __init__(self):
            self.geo = geocoders.GoogleV3()

        @cache("loc_cache.cache", "address", ["loc", "geo_addr"])
        def process_item(self, item, spider):
            try:
                geo_addr, (lat, lng) = self.geo.geocode(item["address"], exactly_one = False)[0]
                item["loc"] = [lng, lat]
                item["geo_addr"] = geo_addr
            except:
                pass
            
            return item



Changelog
=========

0.1.0
initial release

Licence
=======
Copyright 2013 Dimitrios Kouzis-Loukas

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
