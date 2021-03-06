set(SANITIZER_HEADERS
  sanitizer/allocator_interface.h
  sanitizer/asan_interface.h
  sanitizer/common_interface_defs.h
  sanitizer/dfsan_interface.h
  sanitizer/linux_syscall_hooks.h
  sanitizer/lsan_interface.h
  sanitizer/msan_interface.h
  sanitizer/tsan_interface_atomic.h)

set(output_dir ${COMPILER_RT_OUTPUT_DIR}/include)

# Copy compiler-rt headers to the build tree.
set(out_files)
foreach( f ${SANITIZER_HEADERS} )
  set( src ${CMAKE_CURRENT_SOURCE_DIR}/${f} )
  set( dst ${output_dir}/${f} )
  add_custom_command(OUTPUT ${dst}
    DEPENDS ${src}
    COMMAND ${CMAKE_COMMAND} -E copy_if_different ${src} ${dst}
    COMMENT "Copying compiler-rt's ${f}...")
  list(APPEND out_files ${dst})
endforeach( f )

add_custom_target(compiler-rt-headers ALL DEPENDS ${out_files})
add_dependencies(compiler-rt compiler-rt-headers)

# Install sanitizer headers.
install(FILES ${SANITIZER_HEADERS}
  PERMISSIONS OWNER_READ OWNER_WRITE GROUP_READ WORLD_READ
  DESTINATION ${COMPILER_RT_INSTALL_PATH}/include/sanitizer)
