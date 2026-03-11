cmake_minimum_required(VERSION 3.22)

project(PS3100Resonator VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# ─── JUCE path ───
if(DEFINED ENV{JUCE_DIR})
    set(JUCE_DIR $ENV{JUCE_DIR})
elseif(NOT DEFINED JUCE_DIR)
    set(JUCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/JUCE"
        CACHE PATH "Path to the JUCE framework folder")
endif()

if(NOT EXISTS "${JUCE_DIR}/CMakeLists.txt")
    message(FATAL_ERROR
        "\n══════════════════════════════════════════\n"
        "  JUCE not found at: ${JUCE_DIR}\n"
        "  git clone https://github.com/juce-framework/JUCE.git\n"
        "══════════════════════════════════════════\n")
endif()

add_subdirectory(${JUCE_DIR} juce_build)

# ─── Plugin ───
juce_add_plugin(PS3100Resonator
    COMPANY_NAME           "Depattern"
    PLUGIN_MANUFACTURER_CODE Ps3k
    PLUGIN_CODE            Rsnr
    FORMATS                VST3
    PRODUCT_NAME           "PS3100 Resonator"
    IS_SYNTH               FALSE
    NEEDS_MIDI_INPUT       FALSE
    NEEDS_MIDI_OUTPUT      FALSE
    IS_MIDI_EFFECT         FALSE
    EDITOR_WANTS_KEYBOARD_FOCUS FALSE
    COPY_PLUGIN_AFTER_BUILD TRUE
    VST3_COPY_DIR          "$ENV{COMMONPROGRAMFILES}/VST3"
)

target_sources(PS3100Resonator
    PRIVATE
        src/DSP.h
        src/PluginProcessor.h
        src/PluginProcessor.cpp
        src/PluginEditor.h
        src/PluginEditor.cpp
)

target_compile_definitions(PS3100Resonator
    PUBLIC
        JUCE_WEB_BROWSER=0
        JUCE_USE_CURL=0
        JUCE_VST3_CAN_REPLACE_VST2=0
        JUCE_DISPLAY_SPLASH_SCREEN=0
)

target_link_libraries(PS3100Resonator
    PRIVATE
        juce::juce_audio_utils
        juce::juce_dsp
        juce::juce_gui_extra
    PUBLIC
        juce::juce_recommended_config_flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags
)
