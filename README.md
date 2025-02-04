# libs
digital collections aggregation

libraries mainly use openseadragon, mirador etc. to display their digitized documents. iiif is the well known tile source

`/0,0,256,256/256,/0/default.*`

though there are various other formats (some use PDFs i.e salamanca) that follow a scheme somewhat like

`/level_x_y.*`

this repo will find the highest available tile resolution (tif > png > jpg) and then piece together the final image as a TIFF as then LZW compress it using magick. In the case of pdfs it extracts raw ppm using pdfimages then converts these to TIFF via magick see `sh/pdh.sh`

to use it as a CLI `goinstall` set a `$LIBDB` like `/Users/abc/db` then specify the libname or libalias (look in /libs.go) then collection/document ID. Or just use the same pattern with go run. For example,

`libd manchester 123` \
`go run *.go manchester 123`

Some commands will start some long tasks for instance downloading a collection with 100 documents, 100 pages each, 100 tiles per page i.e 100x100x100 = 1,000,000 256x256 tiles. How long will that take? Set `$CONCHANS` for your concurrent routine number and `$CONZZ` for the division of the second of sleep for each channel. We don't want to crash their servers so be nice and consider not setting them to high. From experience 2,4 (2 channels with a 1/4 second sleep per chan) is a good sweet spot and will piece together a document fairly quickly, running at 3, 3 is ok but has crashed some prominent services if run for a little while (not even that long). We appreciate the open sourcing of such high quality images (although some libraries are very stingy with quality i.e. vatican, portuguese national library etc...), so we don't want to crash their servers are revoke the public access to these digital collections. Please don't run something like 10,10 as it will almost 100% bring the tile source servers down or revoke your ip access.

Some services require solving a captcha etc. that usually expires. You can add it as the final term in the relevant command or replace the variable above the relevant libs function.

For PDFs first install https://www.xpdfreader.com/pdfimages-man.html