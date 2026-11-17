cmake_minimum_required(VERSION 3.22)

project(Synth_00 VERSION 1.0.0)

# Point JUCE_DIR at a JUCE checkout, e.g. -DJUCE_DIR=$HOME/Projects/JUCE
set(JUCE_DIR "$ENV{HOME}/Projects/JUCE" CACHE PATH "Path to the JUCE source tree")
add_subdirectory(${JUCE_DIR} JUCE EXCLUDE_FROM_ALL)

juce_add_plugin(Synth_00
    COMPANY_NAME Colatz
    PRODUCT_NAME "Vanity"
    IS_SYNTH TRUE
    NEEDS_MIDI_INPUT TRUE
    PLUGIN_MANUFACTURER_CODE Cola
    PLUGIN_CODE Vnty
    AU_MAIN_TYPE kAudioUnitType_MusicDevice   # instrument: shows up in GarageBand / Logic
    FORMATS AU VST3 Standalone
    COPY_PLUGIN_AFTER_BUILD TRUE)

juce_add_binary_data(Synth_00_Assets
    HEADER_NAME BinaryData.h
    NAMESPACE BinaryData
    SOURCES Assets/background-render.png)

juce_generate_juce_header(Synth_00)

target_sources(Synth_00 PRIVATE
    Source/ADSRComponent.cpp
    Source/ADSRData.cpp
    Source/FilterComponent.cpp
    Source/FilterData.cpp
    Source/FilterTypeSelector.cpp
    Source/OSCComponent.cpp
    Source/OSCData.cpp
    Source/PluginEditor.cpp
    Source/PluginProcessor.cpp
    Source/StyleSheet.cpp
    Source/SynthVoice.cpp
    Source/WaveTypeSelector.cpp)

target_compile_definitions(Synth_00 PUBLIC
    JUCE_WEB_BROWSER=0
    JUCE_USE_CURL=0
    JUCE_VST3_CAN_REPLACE_VST2=0
    JUCE_STRICT_REFCOUNTEDPOINTER=1)

target_link_libraries(Synth_00
    PRIVATE
        Synth_00_Assets
        juce::juce_audio_utils
        juce::juce_dsp
    PUBLIC
        juce::juce_recommended_config_flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags)
