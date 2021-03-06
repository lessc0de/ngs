# ===========================================================================
#
#                            PUBLIC DOMAIN NOTICE
#               National Center for Biotechnology Information
#
#  This software/database is a "United States Government Work" under the
#  terms of the United States Copyright Act.  It was written as part of
#  the author's official duties as a United States Government employee and
#  thus cannot be copyrighted.  This software/database is freely available
#  to the public for use. The National Library of Medicine and the U.S.
#  Government have not placed any restriction on its use or reproduction.
#
#  Although all reasonable efforts have been taken to ensure the accuracy
#  and reliability of the software and data, the NLM and the U.S.
#  Government do not and cannot warrant the performance or results that
#  may be obtained by using this software or data. The NLM and the U.S.
#  Government disclaim all warranties, express or implied, including
#  warranties of performance, merchantability or fitness for any particular
#  purpose.
#
#  Please cite the author in any work or product based on this material.
#
# ===========================================================================

default: std

ifeq (Darwin,$(shell uname -s))
MFLAGS += -mmacosx-version-min=10.6
endif

CC = cc $(MFLAGS) $(CFLAGS)
CXX = c++ $(MFLAGS) $(CPPFLAGS)

# ensure definition of paths, namely
# NCBI_VDB_LIBDIR and NGS_LIBDIR
include $(CURDIR)/Makefile.config

TARGETS =         \
    FragTest      \
    AlignTest     \
    AlignSliceTest \
    RefTest \
    PileupTest \
    FastqTableDump 

# This rule triggers detection of the libraries and headers
# in addition to building the examples
std: $(NCBI_VDB_LIBDIR)/libncbi-vdb-static.a ncbi-headers \
	 $(NGS_LIBDIR)/libngs-c++.a ngs-headers \
	 $(TARGETS)

clean:
	rm -f $(TARGETS) *.o

.PHONY: default std $(TARGETS)

# C++ applications require two NGS libraries:
#  1. ngs-c++ which contains the NGS "front-end" interfaces
#  2. an NGS "back-end" engine such as ncbi-ngs-c++
#     which in turn relies upon ncbi-vdb to access the SRA
#
TEST_LIBS =              \
    -L$(NCBI_VDB_LIBDIR) \
    -L$(NGS_LIBDIR)      \
    -lncbi-ngs-c++       \
    -lngs-c++            \
    -lncbi-ngs-c++       \
    -lncbi-vdb-static    \
    -lngs-adapt-c++      \
    -lpthread            \
    -ldl                 \
    -lm

# FragTest
#  access read fragments
FRAG_TEST_SRC = \
    FragTest.cpp

FragTest: $(FRAG_TEST_SRC)
	$(CXX) -g -o $@ $^ $(TEST_LIBS)

# RefTest ###################
REF_TEST_SRC = \
    RefTest.cpp

RefTest: $(REF_TEST_SRC)
	$(CXX) -g -o $@ $^ $(TEST_LIBS)

# AlignTest #################
#  access alignments
ALIGN_TEST_SRC = \
    AlignTest.cpp

AlignTest: $(ALIGN_TEST_SRC)
	$(CXX) -g -o $@ $^ $(TEST_LIBS)

# AlignSliceTest
#  slice a ReadCollection and output alignments
ALIGNSLICE_TEST_SRC = \
    AlignSliceTest.cpp

AlignSliceTest: $(ALIGNSLICE_TEST_SRC)
	$(CXX) -g -o $@ $^ $(TEST_LIBS)


# PileupTest
#  slice a ReadCollection and produce pileups
PILEUP_TEST_OBJ = \
    PileupTest.cpp

PileupTest: $(PILEUP_TEST_OBJ)
	$(CXX) -g -o $@ $^ $(TEST_LIBS)

# FastqTableDump
#  produce fastq-like table
FASTQ_TABLE_DUMP_OBJ = \
    FastqTableDump.cpp

FastqTableDump: $(FASTQ_TABLE_DUMP_OBJ)
	$(CXX) -g -o $@ $^ $(TEST_LIBS)

# ===========================================================================
#
# example runs

run_frag:  FragTest
	./$^ ERR225922 1 1

run_align: AlignTest
	./$^ ERR225922 1 1

run_align_slice: AlignSliceTest
	./$^ SRR1121656 1 10000 20000


.PHONY: run_align run_align_slice run_frag
