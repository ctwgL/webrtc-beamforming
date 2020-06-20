cmake_minimum_required(VERSION 2.8)

project(wav-beamforming)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

add_subdirectory(gflags)

add_definitions("-DWEBRTC_LINUX -DWEBRTC_POSIX -DWEBRTC_NS_FLOAT")
#-DWEBRTC_UNTRUSTED_DELAY

include_directories(
    webrtc
    webrtc/webrtc/common_audio/signal_processing/include
    webrtc/webrtc/modules/audio_coding/codecs/isac/main/include
    webrtc/webrtc/modules/audio_processing/test/

)

set(WEBRTC_SRC_
    base/buffer.cc
    base/checks.cc
    base/criticalsection.cc
    base/event.cc
    base/event_tracer.cc
    base/logging.cc
    base/platform_file.cc
    base/platform_thread.cc
    base/stringencode.cc
    base/thread_checker_impl.cc
    base/timeutils.cc
    common_audio/audio_converter.cc
    common_audio/audio_ring_buffer.cc
    common_audio/audio_util.cc
    common_audio/blocker.cc
    common_audio/channel_buffer.cc
    common_audio/fft4g.c
    common_audio/fir_filter.cc
    common_audio/fir_filter_sse.cc
    common_audio/lapped_transform.cc
    common_audio/real_fourier.cc
    common_audio/real_fourier_ooura.cc
    common_audio/resampler/push_resampler.cc
    common_audio/resampler/push_sinc_resampler.cc
    common_audio/resampler/resampler.cc
    common_audio/resampler/sinc_resampler.cc
    common_audio/resampler/sinc_resampler_sse.cc
    common_audio/resampler/sinusoidal_linear_chirp_source.cc
    common_audio/ring_buffer.c
    common_audio/signal_processing/auto_correlation.c
    common_audio/signal_processing/auto_corr_to_refl_coef.c
    common_audio/signal_processing/complex_bit_reverse.c
    common_audio/signal_processing/complex_fft.c
    common_audio/signal_processing/copy_set_operations.c
    common_audio/signal_processing/cross_correlation.c
    common_audio/signal_processing/division_operations.c
    common_audio/signal_processing/dot_product_with_scale.c
    common_audio/signal_processing/downsample_fast.c
    common_audio/signal_processing/energy.c
    common_audio/signal_processing/filter_ar.c
    common_audio/signal_processing/filter_ar_fast_q12.c
    common_audio/signal_processing/filter_ma_fast_q12.c
    common_audio/signal_processing/get_hanning_window.c
    common_audio/signal_processing/get_scaling_square.c
    common_audio/signal_processing/ilbc_specific_functions.c
    common_audio/signal_processing/levinson_durbin.c
    common_audio/signal_processing/lpc_to_refl_coef.c
    common_audio/signal_processing/min_max_operations.c
    common_audio/signal_processing/randomization_functions.c
    common_audio/signal_processing/real_fft.c
    common_audio/signal_processing/refl_coef_to_lpc.c
    common_audio/signal_processing/resample_48khz.c
    common_audio/signal_processing/resample_by_2.c
    common_audio/signal_processing/resample_by_2_internal.c
    common_audio/signal_processing/resample.c
    common_audio/signal_processing/resample_fractional.c
    common_audio/signal_processing/spl_init.c
    common_audio/signal_processing/splitting_filter.c
    common_audio/signal_processing/spl_sqrt.c
    common_audio/signal_processing/spl_sqrt_floor.c
    common_audio/signal_processing/sqrt_of_one_minus_x_squared.c
    common_audio/signal_processing/vector_scaling_operations.c
    common_audio/sparse_fir_filter.cc
    common_audio/vad/vad_core.c
    common_audio/vad/vad_filterbank.c
    common_audio/vad/vad_gmm.c
    common_audio/vad/vad_sp.c
    common_audio/vad/webrtc_vad.c
    common_audio/wav_file.cc
    common_audio/wav_header.cc
    common_audio/window_generator.cc
    common_types.cc
    modules/audio_coding/codecs/audio_decoder.cc
    modules/audio_coding/codecs/audio_encoder.cc
    modules/audio_coding/codecs/isac/locked_bandwidth_info.cc
    modules/audio_coding/codecs/isac/main/source/arith_routines.c
    modules/audio_coding/codecs/isac/main/source/arith_routines_hist.c
    modules/audio_coding/codecs/isac/main/source/arith_routines_logist.c
    modules/audio_coding/codecs/isac/main/source/audio_decoder_isac.cc
    modules/audio_coding/codecs/isac/main/source/audio_encoder_isac.cc
    modules/audio_coding/codecs/isac/main/source/bandwidth_estimator.c
    modules/audio_coding/codecs/isac/main/source/crc.c
    modules/audio_coding/codecs/isac/main/source/decode_bwe.c
    modules/audio_coding/codecs/isac/main/source/decode.c
    modules/audio_coding/codecs/isac/main/source/encode.c
    modules/audio_coding/codecs/isac/main/source/encode_lpc_swb.c
    modules/audio_coding/codecs/isac/main/source/entropy_coding.c
    modules/audio_coding/codecs/isac/main/source/fft.c
    modules/audio_coding/codecs/isac/main/source/filterbanks.c
    modules/audio_coding/codecs/isac/main/source/filterbank_tables.c
    modules/audio_coding/codecs/isac/main/source/filter_functions.c
    modules/audio_coding/codecs/isac/main/source/intialize.c
    modules/audio_coding/codecs/isac/main/source/isac.c
    modules/audio_coding/codecs/isac/main/source/lattice.c
    modules/audio_coding/codecs/isac/main/source/lpc_analysis.c
    modules/audio_coding/codecs/isac/main/source/lpc_gain_swb_tables.c
    modules/audio_coding/codecs/isac/main/source/lpc_shape_swb12_tables.c
    modules/audio_coding/codecs/isac/main/source/lpc_shape_swb16_tables.c
    modules/audio_coding/codecs/isac/main/source/lpc_tables.c
    modules/audio_coding/codecs/isac/main/source/pitch_estimator.c
    modules/audio_coding/codecs/isac/main/source/pitch_filter.c
    modules/audio_coding/codecs/isac/main/source/pitch_gain_tables.c
    modules/audio_coding/codecs/isac/main/source/pitch_lag_tables.c
    modules/audio_coding/codecs/isac/main/source/spectrum_ar_model_tables.c
    modules/audio_coding/codecs/isac/main/source/transform.c
    modules/audio_processing/aec/aec_core.cc
    modules/audio_processing/aec/aec_core_sse2.cc
    modules/audio_processing/aec/aec_rdft.cc
    modules/audio_processing/aec/aec_rdft_sse2.cc
    modules/audio_processing/aec/aec_resampler.cc
    modules/audio_processing/aec/echo_cancellation.cc
    modules/audio_processing/aecm/aecm_core.cc
    modules/audio_processing/aecm/aecm_core_c.cc
    modules/audio_processing/aecm/echo_control_mobile.cc
    modules/audio_processing/agc/agc.cc
    modules/audio_processing/agc/agc_manager_direct.cc
    modules/audio_processing/agc/histogram.cc
    modules/audio_processing/agc/legacy/analog_agc.c
    modules/audio_processing/agc/legacy/digital_agc.c
    modules/audio_processing/agc/utility.cc
    modules/audio_processing/audio_buffer.cc
    modules/audio_processing/audio_processing_impl.cc
    modules/audio_processing/beamformer/array_util.cc
    modules/audio_processing/beamformer/covariance_matrix_generator.cc
    modules/audio_processing/beamformer/nonlinear_beamformer.cc
    modules/audio_processing/echo_cancellation_impl.cc
    modules/audio_processing/echo_control_mobile_impl.cc
    modules/audio_processing/gain_control_for_experimental_agc.cc
    modules/audio_processing/gain_control_impl.cc
    modules/audio_processing/high_pass_filter_impl.cc
    modules/audio_processing/intelligibility/intelligibility_enhancer.cc
    modules/audio_processing/intelligibility/intelligibility_utils.cc
    modules/audio_processing/level_estimator_impl.cc
    modules/audio_processing/logging/aec_logging_file_handling.cc
    modules/audio_processing/noise_suppression_impl.cc
    modules/audio_processing/ns/noise_suppression.c
    modules/audio_processing/ns/ns_core.c
    modules/audio_processing/rms_level.cc
    modules/audio_processing/splitting_filter.cc
    modules/audio_processing/three_band_filter_bank.cc
    modules/audio_processing/transient/file_utils.cc
    modules/audio_processing/transient/moving_moments.cc
    modules/audio_processing/transient/transient_detector.cc
    modules/audio_processing/transient/transient_suppressor.cc
    modules/audio_processing/transient/wpd_node.cc
    modules/audio_processing/transient/wpd_tree.cc
    modules/audio_processing/typing_detection.cc
    modules/audio_processing/utility/block_mean_calculator.cc
    modules/audio_processing/utility/delay_estimator.cc
    modules/audio_processing/utility/delay_estimator_wrapper.cc
    modules/audio_processing/vad/gmm.cc
    modules/audio_processing/vad/pitch_based_vad.cc
    modules/audio_processing/vad/pitch_internal.cc
    modules/audio_processing/vad/pole_zero_filter.cc
    modules/audio_processing/vad/standalone_vad.cc
    modules/audio_processing/vad/vad_audio_proc.cc
    modules/audio_processing/vad/vad_circular_buffer.cc
    modules/audio_processing/vad/voice_activity_detector.cc
    modules/audio_processing/voice_detection_impl.cc

    modules/audio_processing/test/test_utils.cc    

    system_wrappers/source/aligned_malloc.cc
    system_wrappers/source/cpu_features.cc
    system_wrappers/source/file_impl.cc
    system_wrappers/source/logging.cc
    system_wrappers/source/metrics_default.cc
    system_wrappers/source/rw_lock.cc
    system_wrappers/source/rw_lock_posix.cc
    system_wrappers/source/trace_impl.cc
    system_wrappers/source/trace_posix.cc
    
)

function(prepend_path var prefix)
   set(listVar "")
   foreach(f ${ARGN})
      list(APPEND listVar "${prefix}/${f}")
   endforeach(f)
   set(${var} "${listVar}" PARENT_SCOPE)
endfunction(prepend_path)

prepend_path(WEBRTC_SRC webrtc/webrtc ${WEBRTC_SRC_})

# add_executable(webrtc-audioproc webrtc-audioproc.cpp ${WEBRTC_SRC})
add_executable(webrtc-bf nonlinear_beamformer_test.cc ${WEBRTC_SRC})
# target_link_libraries(webrtc-audioproc gflags pthread)
target_link_libraries(webrtc-bf gflags pthread)
SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb") 
SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall") 
