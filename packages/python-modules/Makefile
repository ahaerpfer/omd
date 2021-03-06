include ../../Makefile.omd

NAME     = python-modules
SHELL    = /bin/bash
FREETDS  = $(shell readlink -m ../freetds/tmpinstall$(OMD_ROOT))

# This is where the python modules are going to be installed to
LOCALPYTHONDIR=$(DESTDIR)$(OMD_ROOT)/lib/python

MODULES =
MODULES += setuptools-24.2.0.tar.gz
MODULES += simplejson-3.8.2.tar.gz
MODULES += texttable-0.8.7.tar.gz
MODULES += ordereddict-1.1.tar.gz
MODULES += weakrefset-1.0.0.tar.gz
MODULES += pyexcel-io-0.3.2.tar.gz
MODULES += pyexcel-0.4.3.tar.gz
MODULES += odfpy-1.3.4.tar.gz
MODULES += pyexcel-ods-0.3.0.tar.gz
MODULES += jdcal-1.3.tar.gz
MODULES += et_xmlfile-1.0.1.tar.gz
MODULES += openpyxl-2.4.2.tar.gz
MODULES += importlib-1.0.4.zip
MODULES += argparse-1.4.0.tar.gz
MODULES += future-0.16.0.tar.gz
MODULES += xlwt-future-0.8.0.tar.gz
MODULES += xlrd-1.0.0.tar.gz
MODULES += xlwt-1.2.0.tar.gz
MODULES += pyexcel-xlsx-0.3.0.tar.gz
MODULES += pyexcel-xls-0.3.1.tar.gz
MODULES += MarkupSafe-0.23.tar.gz
MODULES += Jinja2-2.8.tar.gz
MODULES += wstools-0.4.tar.gz
MODULES += fpconst-0.7.2.tar.gz
MODULES += docutils-0.12.tar.gz
MODULES += SOAPpy-0.12.5.tar.gz
MODULES += PyYAML-3.10.tar.gz
MODULES += ecdsa-0.13.tar.gz
MODULES += gearman-2.0.2.tar.gz
MODULES += six-1.10.0.tar.gz
MODULES += orderedmultidict-0.7.5.tar.gz
MODULES += pycrypto-2.6.1.tar.gz
MODULES += paramiko-1.18.2.tar.gz
MODULES += pyasn1-0.2.3.tar.gz
MODULES += ply-3.10.tar.gz
MODULES += pysmi-0.1.1.tar.gz
MODULES += pycryptodome-3.4.5.tar.gz
MODULES += pysnmp-4.3.5.tar.gz
MODULES += pywbem-0.7.0.tar.gz
MODULES += requests-2.10.0.tar.gz
MODULES += prometheus_client-0.0.17.tar.gz

# More datasources for coshsh
MODULES += MySQL-python-1.2.5.zip
MODULES += pymssql-2.1.3.tar.gz

# DataScryer:
#MODULES += functools32-3.2.3-2.tar.gz
#MODULES += vcversioner-2.16.0.0.tar.gz
#MODULES += jsonschema-2.5.1.tar.gz


build:
	mkdir -p dest
	set -e ; cd dest && \
	    export PYTHONPATH=$$PYTHONPATH:$(LOCALPYTHONDIR); \
	    if [ `uname -m` = 'armv7l' ]; then \
		export PYMSSQL_DONT_BUILD_WITH_BUNDLED_FREETDS=1; \
	    else \
		export PYMSSQL_BUILD_WITH_BUNDLED_FREETDS=1; \
	    fi; \
	    unset http_proxy; \
	    unset https_proxy; \
	    unset ftp_proxy; \
	    for M in $(MODULES); do \
	        if echo $$M | grep .tar.gz; then \
	            tar xzf ../src/$$M ; \
	        else \
	            unzip -o ../src/$$M ; \
	        fi; \
	        MDIR=$$(echo $$M | sed -e 's/.tar.gz//g' -e 's/.zip//g') ; \
	        cd $$MDIR; \
	        echo PYTHONBUILD $$M ; \
		if [ `uname -m` = 'armv7l' ] && [[ "$$M" =~ .*pymssql.* ]]; then \
	            python setup.py build_ext -I $(FREETDS)/include -L $(FREETDS)/lib ; \
		else \
	            python setup.py build ; \
		fi ; \
	        cd .. ; \
	    done

install:
	mkdir -p $(LOCALPYTHONDIR)
	set -e ; cd dest && \
	    export PYTHONPATH=$$PYTHONPATH:$(LOCALPYTHONDIR); \
	    unset http_proxy; \
	    unset https_proxy; \
	    unset ftp_proxy; \
	    for M in $(MODULES); do \
	        MDIR=$$(echo $$M | sed -e 's/.tar.gz//g' -e 's/.zip//g') ; \
	        cd $$MDIR ; \
	        echo PYTHONINSTALL $$M ; \
	        python setup.py install --home=$(DESTDIR)$(OMD_ROOT) \
	            --prefix='' \
	            --install-platlib=$(LOCALPYTHONDIR) \
	            --install-purelib=$(LOCALPYTHONDIR) ; \
	        cd .. ; \
	    done

skel:

clean:
	rm -rf dest
