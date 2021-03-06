file(GLOB HEADERS "include/eos/chain/*.hpp")

## SORT .cpp by most likely to change / break compile
add_library( eos_chain
             chain_controller.cpp
             fork_database.cpp

             transaction.cpp
             block.cpp

             get_config.cpp

             block_log.cpp
             BlockchainConfiguration.cpp

             types.cpp

             ${HEADERS}
           )

target_link_libraries( eos_chain fc chainbase eos_types wren )
target_include_directories( eos_chain
                            PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include" "${CMAKE_CURRENT_BINARY_DIR}/include" )

if(MSVC)
  set_source_files_properties( db_init.cpp db_block.cpp database.cpp block_log.cpp PROPERTIES COMPILE_FLAGS "/bigobj" )
endif(MSVC)

INSTALL( TARGETS
   eos_chain

   RUNTIME DESTINATION bin
   LIBRARY DESTINATION lib
   ARCHIVE DESTINATION lib
)
INSTALL( FILES ${HEADERS} DESTINATION "include/eos/chain" )
