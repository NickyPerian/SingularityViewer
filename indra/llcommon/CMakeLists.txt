# -*- cmake -*-

project(llcommon)

include(Cwdebug)
include(00-Common)
include(LLCommon)
include(APR)
include(Linking)

include_directories(
    ${EXPAT_INCLUDE_DIRS}
    ${LLCOMMON_INCLUDE_DIRS}
    ${ZLIB_INCLUDE_DIRS}
    )

set(llcommon_SOURCE_FILES
	aiaprpool.cpp
	imageids.cpp
	indra_constants.cpp
    llapp.cpp
    llapr.cpp
    llassettype.cpp
	llavatarname.cpp
    llbase32.cpp
    llbase64.cpp
    llcommon.cpp
    llcommonutils.cpp
    llcrc.cpp
    llcriticaldamp.cpp
    llcursortypes.cpp
    lldate.cpp
    lldictionary.cpp
    llerror.cpp
    llerrorthread.cpp
    llevent.cpp
    lleventtimer.cpp
    llfasttimer.cpp
    llfile.cpp
    llfindlocale.cpp
    llfixedbuffer.cpp
    llformat.cpp
    llframetimer.cpp
    llheartbeat.cpp
    llinstancetracker.cpp
    llindraconfigfile.cpp
    llliveappconfig.cpp
    lllivefile.cpp
    lllog.cpp
    llmd5.cpp
    llmemory.cpp
    llmemorystream.cpp
    llmetrics.cpp
    llmortician.cpp
    lloptioninterface.cpp
    llprocesslauncher.cpp
    llprocessor.cpp
    llqueuedthread.cpp
    llrand.cpp
    llrun.cpp
    llsd.cpp
    llsdserialize.cpp
    llsdserialize_xml.cpp
    llsdutil.cpp
    llsecondlifeurls.cpp
    llstat.cpp
    llstacktrace.cpp
    llstreamtools.cpp
    llstring.cpp
    llstringtable.cpp
    llsys.cpp
    llthread.cpp
    lltimer.cpp
    lluri.cpp
    lluuid.cpp
    llworkerthread.cpp
    metaclass.cpp
    metaproperty.cpp
    reflective.cpp
    timing.cpp
    u64.cpp
    )
    
set(llcommon_HEADER_FILES
    CMakeLists.txt

    aiaprpool.h
    aithreadsafe.h
    bitpack.h
    ctype_workaround.h
    doublelinkedlist.h
    imageids.h
    indra_constants.h
    linden_common.h
    linked_lists.h
    llagentconstants.h
    llavatarname.h
    llapp.h
    llapr.h
    llassettype.h
    llassoclist.h
    llavatarconstants.h
    llbase32.h
    llbase64.h
    llboost.h
    llchat.h
    llclickaction.h
    llcommon.h
    llcommonutils.h
    llcrc.h
    llcriticaldamp.h
    llcursortypes.h
    lldarray.h
    lldarrayptr.h
    lldate.h
    lldefs.h
    lldeleteutils.h
    lldepthstack.h
    lldictionary.h
    lldlinked.h
    lldqueueptr.h
    llendianswizzle.h
    llenum.h
    llerror.h
    llerrorcontrol.h
    llerrorlegacy.h
    llerrorthread.h
    llevent.h
    lleventemitter.h
    llextendedstatus.h
    lleventtimer.h
    llfasttimer.h
    llfile.h
    llfindlocale.h
    llfixedbuffer.h
    llformat.h
    llframetimer.h
    llhash.h
    llheartbeat.h
    llhttpstatuscodes.h
    llindexedqueue.h
    llinstancetracker.h
    llindraconfigfile.h
    llkeythrottle.h
    lllinkedqueue.h
    llliveappconfig.h
    lllivefile.h
    lllocalidhashmap.h
    lllog.h
    lllslconstants.h
    llmap.h
    llmd5.h
    llmemory.h
    llmemorystream.h
    llmemtype.h
    llmetrics.h
    llmortician.h
    llnametable.h
    lloptioninterface.h
    llpreprocessor.h
    llpriqueuemap.h
    llprocesslauncher.h
    llprocessor.h
    llptrskiplist.h
    llptrskipmap.h
    llqueuedthread.h
    llrand.h
    llrun.h
    llscopedvolatileaprpool.h
    llsd.h
    llsdserialize.h
    llsdserialize_xml.h
    llsdutil.h
    llsecondlifeurls.h
    llsimplehash.h
    llskiplist.h
    llskipmap.h
    llstack.h
    llstacktrace.h
    llstat.h
    llstatenums.h
    llstl.h
    llstreamtools.h
    llstrider.h
    llstring.h
    llstringtable.h
    llsys.h
    llthread.h
    lltimer.h
    lluri.h
    lluuid.h
    lluuidhashmap.h
    llversionserver.h
    llversionviewer.h
    llworkerthread.h
    metaclass.h
    metaclasst.h
    metaproperty.h
    metapropertyt.h
    reflective.h
    reflectivet.h
    roles_constants.h
    stdenums.h
    stdtypes.h
    string_table.h
    timer.h
    timing.h
    u64.h
    )

set_source_files_properties(${llcommon_HEADER_FILES}
                            PROPERTIES HEADER_FILE_ONLY TRUE)

list(APPEND llcommon_SOURCE_FILES ${llcommon_HEADER_FILES})

add_library (llcommon SHARED ${llcommon_SOURCE_FILES})
add_dependencies(llcommon prepare)
target_link_libraries(
    llcommon
    ${APRUTIL_LIBRARIES}
    ${APR_LIBRARIES}
    ${EXPAT_LIBRARIES}
    ${ZLIB_LIBRARIES}
    ${WINDOWS_LIBRARIES}
    ${CWDEBUG_LIBRARIES}
    )

if (LINUX)
    # When linking with llcommon later, we do not want to link with cwdebug.a again.
    set_property(TARGET llcommon PROPERTY LINK_INTERFACE_LIBRARIES "-lapr-1 -laprutil-1 -lz")
endif (LINUX)

if (DARWIN)
    # Don't embed a full path in the library's install name
    set_target_properties(
        llcommon
        PROPERTIES
        BUILD_WITH_INSTALL_RPATH 1
        INSTALL_NAME_DIR "@executable_path/../Resources"
      )
endif (DARWIN)
