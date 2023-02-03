add_llvm_library(clangInterOp OBJECT
  InterOp.cpp
 )

if (USE_CLING)
  ADD_LIBRARY(InterOp::ClingInterpreter STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::ClingInterpreter PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libclingInterpreter.a)

  ADD_LIBRARY(InterOp::ClingUtils STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::ClingUtils PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libclingUtils.a)

  ADD_LIBRARY(InterOp::ClangBasic STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::ClangBasic PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libclangBasic.a)
  
  ADD_LIBRARY(InterOp::ClangAST STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::ClangAST PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libclangAST.a)

  ADD_LIBRARY(InterOp::ClangSema STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::ClangSema PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libclangSema.a)

  ADD_LIBRARY(InterOp::ClangLex STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::ClangLex PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libclangLex.a)

  ADD_LIBRARY(InterOp::ClangDriver STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::ClangDriver PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libclangDriver.a)

  ADD_LIBRARY(InterOp::ClangFrontend STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::ClangFrontend PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libclangFrontend.a)

  ADD_LIBRARY(InterOp::ClangCodeGen STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::ClangCodeGen PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libclangCodeGen.a)

  ADD_LIBRARY(InterOp::LLVMCore STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::LLVMCore PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libLLVMCore.a)

  ADD_LIBRARY(InterOp::LLVMJITLink STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::LLVMJITLink PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libLLVMJITLink.a)

  ADD_LIBRARY(InterOp::LLVMipo STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::LLVMipo PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libLLVMipo.a)

  ADD_LIBRARY(InterOp::LLVMSupport STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::LLVMSupport PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libLLVMSupport.a)

  ADD_LIBRARY(InterOp::LLVMNVPTXDesc STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::LLVMNVPTXDesc PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libLLVMNVPTXDesc.a)

  ADD_LIBRARY(InterOp::LLVMTarget STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::LLVMTarget PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libLLVMTarget.a)
  
  ADD_LIBRARY(InterOp::LLVMTransformUtils STATIC IMPORTED)
  SET_TARGET_PROPERTIES(InterOp::LLVMTransformUtils PROPERTIES IMPORTED_LOCATION ${Cling_DIR}/../../libLLVMTransformUtils.a)

  if (APPLE)
    set(_cling_link_flags -Wl,-force_load 
        $<TARGET_FILE:InterOp::ClingInterpreter> 
        $<TARGET_FILE:InterOp::ClingUtils> 
        $<TARGET_FILE:InterOp::ClangBasic>
        $<TARGET_FILE:InterOp::ClangAST>
        $<TARGET_FILE:InterOp::ClangSema> 
        $<TARGET_FILE:InterOp::ClangLex>
        $<TARGET_FILE:InterOp::LLVMCore>
        $<TARGET_FILE:InterOp::LLVMJITLink> -Wl)
  elseif(MSVC)
    set(_cling_link_flags "-WHOLEARCHIVE:" $<TARGET_FILE:InterOp::ClingInterpreter> $<TARGET_FILE:InterOp::ClingUtils> $<TARGET_FILE:InterOp::ClangBasic> $<TARGET_FILE:InterOp::ClangAST> $<TARGET_FILE:InterOp::ClangSema> $<TARGET_FILE:InterOp::ClangLex> $<TARGET_FILE:InterOp::LLVMCore> $<TARGET_FILE:InterOp::LLVMJITLink>)
  else()
    set(_cling_link_flags -Wl,--whole-archive
        $<TARGET_FILE:InterOp::ClingInterpreter>
        $<TARGET_FILE:InterOp::ClingUtils>
        $<TARGET_FILE:InterOp::ClangBasic>
        $<TARGET_FILE:InterOp::ClangAST>
        $<TARGET_FILE:InterOp::ClangSema>
        $<TARGET_FILE:InterOp::ClangLex>
        $<TARGET_FILE:InterOp::ClangDriver>
        $<TARGET_FILE:InterOp::ClangFrontend>
        $<TARGET_FILE:InterOp::ClangCodeGen>
        $<TARGET_FILE:InterOp::LLVMCore>
        $<TARGET_FILE:InterOp::LLVMJITLink>
        $<TARGET_FILE:InterOp::LLVMNVPTXDesc>
        $<TARGET_FILE:InterOp::LLVMTarget>
        # $<TARGET_FILE:InterOp::LLVMipo>
        # $<TARGET_FILE:InterOp::LLVMSupport>
        -Wl,--no-whole-archive)
  endif()
  target_link_libraries(clangInterOp PRIVATE ${_cling_link_flags})
endif()

# if (APPLE)
#   set(_interop_link_flags -Wl,-force_load $<TARGET_FILE:InterOp::libInterOp> -Wl)
# elseif(MSVC)
#   set(_interop_link_flags "-WHOLEARCHIVE:" $<TARGET_FILE:InterOp::libInterOp>)
# else()
#   set(_interop_link_flags -Wl,--whole-archive -lclingInterpreter -Wl,--no-whole-archive)
# # endif()
# target_link_libraries(clangInterOp PUBLIC ${_interop_link_flags})

# target_link_libraries(clangInterOp
#   PRIVATE
#   clingInterpreter
#   clangAST
#   clangBasic
#   clangLex
#   LLVMCore
#   LLVMSupport
# )
