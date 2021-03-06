add_package()

# CMake does not do dependency tracking if files are listed by a GLOB operation.
# So, we have to list the files individually as we want them to be tracked as
# build inputs.
filegroup(
  NAME
    builtins_pytd
  SRCS
    pytd/builtins/2/__builtin__.pytd
    pytd/builtins/2/array.pytd
    pytd/builtins/2/attr/__init__.pytd
    pytd/builtins/2/attr/_compat.pytd
    pytd/builtins/2/attr/_config.pytd
    pytd/builtins/2/attr/_funcs.pytd
    pytd/builtins/2/attr/_make.pytd
    pytd/builtins/2/attr/converters.pytd
    pytd/builtins/2/attr/exceptions.pytd
    pytd/builtins/2/attr/filters.pytd
    pytd/builtins/2/attr/setters.pytd
    pytd/builtins/2/attr/validators.pytd
    pytd/builtins/2/mypy_extensions.pytd
    pytd/builtins/2/numpy/__init__.pytd
    pytd/builtins/2/protocols.pytd
    pytd/builtins/2/typing.pytd

    pytd/builtins/2and3/array.pytd
    pytd/builtins/2and3/attr/__init__.pytd
    pytd/builtins/2and3/attr/_compat.pytd
    pytd/builtins/2and3/attr/_config.pytd
    pytd/builtins/2and3/attr/_funcs.pytd
    pytd/builtins/2and3/attr/_make.pytd
    pytd/builtins/2and3/attr/converters.pytd
    pytd/builtins/2and3/attr/exceptions.pytd
    pytd/builtins/2and3/attr/filters.pytd
    pytd/builtins/2and3/attr/setters.pytd
    pytd/builtins/2and3/attr/validators.pytd
    pytd/builtins/2and3/mypy_extensions.pytd
    pytd/builtins/2and3/numpy/__init__.pytd
    pytd/builtins/2and3/protocols.pytd
    pytd/builtins/2and3/typing.pytd

    pytd/builtins/3/__builtin__.pytd
    pytd/builtins/3/array.pytd
    pytd/builtins/3/attr/__init__.pytd
    pytd/builtins/3/attr/_compat.pytd
    pytd/builtins/3/attr/_config.pytd
    pytd/builtins/3/attr/_funcs.pytd
    pytd/builtins/3/attr/_make.pytd
    pytd/builtins/3/attr/converters.pytd
    pytd/builtins/3/attr/exceptions.pytd
    pytd/builtins/3/attr/filters.pytd
    pytd/builtins/3/attr/setters.pytd
    pytd/builtins/3/attr/validators.pytd
    pytd/builtins/3/builtins.pytd
    pytd/builtins/3/mypy_extensions.pytd
    pytd/builtins/3/numpy/__init__.pytd
    pytd/builtins/3/protocols.pytd
    pytd/builtins/3/typing.pytd

    pytd/stdlib/2/_ctypes.pytd
    pytd/stdlib/2/collections.pytd
    pytd/stdlib/2/dummy_thread.pytd
    pytd/stdlib/2/encodings.pytd

    pytd/stdlib/2and3/_ctypes.pytd
    pytd/stdlib/2and3/collections.pytd
    pytd/stdlib/2and3/dummy_thread.pytd
    pytd/stdlib/2and3/encodings.pytd

    pytd/stdlib/3/_ctypes.pytd
    pytd/stdlib/3/collections.pytd
    pytd/stdlib/3/dummy_thread.pytd
    pytd/stdlib/3/encodings.pytd
    pytd/stdlib/3/enum.pytd
)

toplevel_py_binary(
  NAME
    pytype-single
  SRCS
    single.py
  MAIN
    single.py
  DEPS
    .config
    .io
    .libvm
)

py_library(
  NAME
    metrics
  SRCS
    metrics.py
)

py_library(
  NAME
    utils
  SRCS
    compat.py
    datatypes.py
    file_utils.py
    module_utils.py
    pytype_source_utils.py
    utils.py
  DEPS
    .builtins_pytd
)

py_library(
  NAME
    debug
  SRCS
    debug.py
  DEPS
    .utils
)

py_library(
  NAME
    config
  SRCS
    config.py
  DEPS
    .errors
    .libvm
)

py_library(
  NAME
    errors
  SRCS
    errors.py
  DEPS
    .libvm
)

py_library(
  NAME
    io
  SRCS
    io.py
  DEPS
    .analyze
    .errors
    .libvm
)

py_library(
  NAME
    analyze
  SRCS
    analyze.py
  DEPS
    .libvm
    .output
    .vm
    pytype.overlays.typing_overlay
)

py_library(
  NAME
    vm
  SRCS
    vm.py
  DEPS
    .annotations_util
    .convert
    .libvm
    .matcher
    .overlay_dict
)

py_library(
  NAME
    annotations_util
  SRCS
    annotations_util.py
  DEPS
    .libvm
    pytype.overlays.typing_overlay
)

py_library(
  NAME
    convert
  SRCS
    convert.py
  DEPS
    .libvm
    .output
    pytype.overlays.typing_overlay
)

py_library(
  NAME
    matcher
  SRCS
    matcher.py
  DEPS
    .libvm
    pytype.overlays.dataclass_overlay
    pytype.overlays.typing_overlay
)

py_library(
  NAME
    output
  SRCS
    output.py
  DEPS
    .libvm
    pytype.overlays.dataclass_overlay
    pytype.overlays.typing_overlay
)

py_library(
  NAME
    overlay_dict
  SRCS
    overlay_dict.py
  DEPS
    pytype.overlays.abc_overlay
    pytype.overlays.asyncio_types_overlay
    pytype.overlays.attr_overlay
    pytype.overlays.collections_overlay
    pytype.overlays.dataclass_overlay
    pytype.overlays.flax_overlay
    pytype.overlays.future_overlay
    pytype.overlays.six_overlay
    pytype.overlays.subprocess_overlay
    pytype.overlays.sys_overlay
    pytype.overlays.typing_extensions_overlay
    pytype.overlays.typing_overlay
)

py_library(
  NAME
    libvm
  SRCS
    __version__.py
    abstract.py
    attribute.py
    blocks.py
    compare.py
    convert_structural.py
    directors.py
    imports_map_loader.py
    metaclass.py
    overlay.py
    overlay_utils.py
    special_builtins.py
    state.py
  DEPS
    .abstract_utils
    .debug
    .function
    .metrics
    .mixin
    .pyc
    .pytd
    .utils
)

py_library(
  NAME
    abstract_utils
  SRCS
    abstract_utils.py
  DEPS
    .cfg_utils
    .pyc
    .pytd_for_parser
    .utils
)

py_library(
  NAME
    function
  SRCS
    function.py
  DEPS
    .abstract_utils
    .pytd_for_parser
    .utils
)

py_library(
  NAME
    mixin
  SRCS
    mixin.py
  DEPS
    .abstract_utils
    .function
    .pytd_for_parser
    .utils
    pytype.typegraph.cfg
)

py_library(
  NAME
    pytd
  SRCS
    load_pytd.py
    pytd/parse/builtins.py
    pytd/serialize_ast.py
    pytd/typeshed.py
  DEPS
    .parser
    .pytd_for_parser
    .utils
)

py_library(
    NAME
      tools
    SRCS
      tools/arg_parser.py
      tools/config.py
      tools/environment.py
      tools/runner.py
      tools/tool_utils.py
    DEPS
      .config
      .libvm
      .utils
)

py_library(
    NAME
      analyze_project
    SRCS
      tools/analyze_project/config.py
      tools/analyze_project/environment.py
      tools/analyze_project/parse_args.py
      tools/analyze_project/pytype_runner.py
    DEPS
      .config
      .io
      .tools
      .utils
)

# This library is disabled because cfg.py can conflict with the typegraph.cfg
# extension module.
# py_library(
#   NAME
#     cfg_py
#   SRCS
#     typegraph/cfg.py
#   DEPS
#     .debug
#     .metrics
# )

py_library(
  NAME
    cfg_utils
  SRCS
    typegraph/cfg_utils.py
  DEPS
    pytype.typegraph.cfg
)

py_library(
  NAME
    preconditions
  SRCS
    pytd/parse/preconditions.py
  DEPS
    .utils
)

py_library(
  NAME
    node_c
  SRCS
    pytd/parse/node.py
)

py_library(
  NAME
    node
  DEPS
    .metrics
    .node_c
    .preconditions
)

py_library(
  NAME
    pytd_defs
  SRCS
    pytd/pytd.py
  DEPS
    .node
    .preconditions
)

py_library(
  NAME
    pytd_visitors
  SRCS
    pytd/pytd_visitors.py
  DEPS
    .cfg_utils
    .parser_constants
    .pytd_defs
)

py_library(
  NAME
    pytd_utils
  SRCS
    pytd/pytd_utils.py
  DEPS
    .pytd_defs
    .pytd_visitors
    .utils
)

py_library(
  NAME
    parser_constants
  SRCS
    pytd/parse/parser_constants.py
)

py_library(
  NAME
    pytd_for_parser
  SRCS
    pytd/abc_hierarchy.py
    pytd/booleq.py
    pytd/escape.py
    pytd/mro.py
    pytd/optimize.py
    pytd/pep484.py
    pytd/slots.py
    pytd/transforms.py
    pytd/type_match.py
    pytd/visitors.py
  DEPS
    .builtins_pytd  # TODO(b/159043305): Move this into a data dependency
    .debug
    .metrics
    .node
    .parser_constants
    .pytd_defs
    .pytd_utils
    .pytd_visitors
    .utils
    pytype.typegraph.cfg
)

py_library(
  NAME
    pyc
  SRCS
    pyc/compile_bytecode.py
    pyc/loadmarshal.py
    pyc/magic.py
    pyc/opcodes.py
    pyc/pyc.py
  DEPS
    .utils
)

py_library(
  NAME
    parser
  SRCS
    pyi/parser.py
  DEPS
    .pytd_for_parser
    .utils
    pytype.pyi.parser_ext
)

py_test(
  NAME
    config_test
  SRCS
    config_test.py
  DEPS
    .config
    .utils
)

py_test(
  NAME
    compat_test
  SRCS
    compat_test.py
  DEPS
    pytype.tests.test_base
)

py_test(
  NAME
    datatypes_test
  SRCS
    datatypes_test.py
  DEPS
    pytype.tests.test_base
)

py_test(
  NAME
    io_test
  SRCS
    io_test.py
  DEPS
    .io
    pytype.tests.test_base
)

py_test(
  NAME
    file_utils_test
  SRCS
    file_utils_test.py
  DEPS
    pytype.tests.test_base
)

py_test(
  NAME
    module_utils_test
  SRCS
    module_utils_test.py
  DEPS
    pytype.tests.test_base
)

py_test(
  NAME
    pytype_source_utils_test
  SRCS
    pytype_source_utils_test.py
  DEPS
    .utils
)

py_test(
  NAME
    utils_test
  SRCS
    utils_test.py
  DEPS
    pytype.tests.test_base
)

py_test(
  NAME
    abstract_test
  SRCS
    abstract_test.py
  DEPS
    .config
    .libvm
    .vm
    pytype.tests.test_base
)

py_test(
  NAME
    abstract_utils_test
  SRCS
    abstract_utils_test.py
  DEPS
    .abstract_utils
    .config
    .libvm
    .vm
    pytype.tests.test_base
)

py_test(
  NAME
    compare_test
  SRCS
    compare_test.py
  DEPS
    .config
    .libvm
    .vm
    pytype.tests.test_base
)

py_test(
  NAME
    mixin_test
  SRCS
    mixin_test.py
  DEPS
    .mixin
)

py_test(
  NAME
    matcher_test
  SRCS
    matcher_test.py
  DEPS
    .config
    .libvm
    .vm
    pytype.tests.test_base
)

py_test(
  NAME
    convert_test
  SRCS
    convert_test.py
  DEPS
    .config
    .libvm
    .vm
    pytype.tests.test_base
)

py_test(
  NAME
    attribute_test
  SRCS
    attribute_test.py
  DEPS
    .config
    .libvm
    .vm
    pytype.tests.test_base
)

py_test(
  NAME
    blocks_test
  SRCS
    blocks_test.py
  DEPS
    pytype.tests.test_base
)

py_test(
  NAME
    vm_test
  SRCS
    vm_test.py
  DEPS
    .vm
    pytype.tests.test_base
)

py_test(
  NAME
    directors_test
  SRCS
    directors_test.py
  DEPS
    .errors
    .libvm
    pytype.tests.test_base
)

py_test(
  NAME
    load_pytd_test
  SRCS
    load_pytd_test.py
  DEPS
    .builtins_pytd
    .libvm
    pytype.tests.test_base
)

py_test(
  NAME
    imports_map_loader_test
  SRCS
    imports_map_loader_test.py
  DEPS
    .builtins_pytd
    .libvm
)

py_test(
  NAME
    metrics_test
  SRCS
    metrics_test.py
  DEPS
    .metrics
)

py_test(
  NAME
    state_test
  SRCS
    state_test.py
  DEPS
    .libvm
)

py_library(
  NAME
    pytdtest
  SRCS
    pytd/parse/parser_test_base.py
  DEPS
    .pytd
    pytype.tests.test_base
)

py_test(
  NAME
    debug_test
  SRCS
    debug_test.py
  DEPS
    .debug
    .pytd
)

py_test(
  NAME
    typegraph_metrics_test
  SRCS
    typegraph_metrics_test.py
  DEPS
    .config
    .libvm
    pytype.typegraph.cfg
    pytype.tests.test_base
)

add_subdirectory(ast)
add_subdirectory(overlays)
add_subdirectory(pyc)
add_subdirectory(pyi)
add_subdirectory(pytd)
add_subdirectory(tests)
add_subdirectory(tools)
add_subdirectory(typegraph)
