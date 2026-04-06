find_package(Python3 REQUIRED COMPONENTS Interpreter)

set(VENV_DIR "${CMAKE_CURRENT_SOURCE_DIR}/.venv")
set(REQUIREMENTS_PATH "${CMAKE_CURRENT_SOURCE_DIR}/requirements.txt")

set(TEST_CDM_FILE_NAME "test_cdm")
if (WIN32)
    set(TEST_CDM_FILE_NAME "${TEST_CDM_FILE_NAME}.py")
    set(VENV_PYTHON_EXECUTABLE "${VENV_DIR}/Scripts/python.exe")
else()
    set(VENV_PYTHON_EXECUTABLE "${VENV_DIR}/bin/python3")
endif()

# Set up venv
execute_process(
                   COMMAND ${Python3_EXECUTABLE} -m venv ${VENV_DIR}
                   COMMAND_ECHO STDOUT
                   COMMAND_ERROR_IS_FATAL ANY)

execute_process(
                   COMMAND ${VENV_PYTHON_EXECUTABLE} -m pip install -r ${REQUIREMENTS_PATH}
                   COMMAND_ECHO STDOUT
                   COMMAND_ERROR_IS_FATAL ANY)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/test_cdm/test_cdm.py.in ${LLVM_BINARY_DIR}/bin/${TEST_CDM_FILE_NAME})
