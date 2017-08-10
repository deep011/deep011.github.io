---
layout: post
title: RocksDB(5.0.1) Options
---

## DBOptions

+ **info_log_level**：
+ **write_thread_max_yield_usec**：
+ **write_thread_slow_yield_usec**：
+ **fail_if_options_file_error**：
+ **stats_dump_period_sec**：
+ **max_total_wal_size**：
+ **wal_recovery_mode**：
+ **wal_bytes_per_sync**：
+ **max_manifest_file_size**：
+ **avoid_flush_during_shutdown**：
+ **bytes_per_sync**：
+ **max_subcompactions**：
+ **WAL_ttl_seconds**：
+ **wal_dir**：
+ **enable_write_thread_adaptive_yield**：
+ **manifest_preallocation_size**：
+ **log_file_time_to_roll**：
+ **recycle_log_file_num**：
+ **allow_concurrent_memtable_write**：
+ **keep_log_file_num**：
+ **db_write_buffer_size**：
+ **dump_malloc_stats**：
+ **table_cache_numshardbits**：
+ **max_open_files**：最多可以缓存多少file descriptors。
+ **base_background_compactions**：
+ **max_background_compactions**：最大的后台compaction线程数。
+ **db_log_dir**：
+ **use_fsync**：
+ **use_adaptive_mutex**：
+ **writable_file_max_buffer_size**：
+ **max_background_flushes**：最大的后台磁盘flush线程数。
+ **compaction_readahead_size**：
+ **skip_stats_update_on_db_open**：
+ **skip_log_error_on_recovery**：
+ **new_table_reader_for_compaction_inputs**：
+ **error_if_exists**：
+ **enable_thread_tracking**：
+ **delete_obsolete_files_period_micros**：
+ **random_access_max_buffer_size**：
+ **is_fd_close_on_exec**：
+ **disable_data_sync**：
+ **WAL_size_limit_MB**：
+ **create_missing_column_families**：
+ **paranoid_checks**：
+ **disableDataSync**：
+ **create_if_missing**：
+ **access_hint_on_compaction_start**：
+ **max_log_file_size**：
+ **allow_2pc**：
+ **use_direct_reads**：
+ **allow_mmap_writes**：
+ **delayed_write_rate**：
+ **max_file_opening_threads**：
+ **allow_fallocate**：
+ **avoid_flush_during_recovery**：
+ **allow_os_buffer**：是否允许文件缓存到OS Cache。
+ **allow_mmap_reads**：
+ **advise_random_on_open**：

## CFOptions

+ **compaction_filter_factory**：
+ **memtable_factory**：
+ **compression**：sst文件压缩算法，可以是none,snappy,zlib,bzip2,lz4,lz4hc,xpress。默认为不压缩。
+ **bottommost_compression**：包含sst文件的最大level层使用的压缩算法。
+ **compression_per_level**：用于控制各Level的压缩算法。通常level 0和level 1不压缩（或者快压缩算法），高Level使用慢压缩算法。
+ **max_sequential_skip_in_iterations**：
+ **max_bytes_for_level_multiplier**：Level N+1 的最大容量是Level N 最大容量的多少倍。默认是10。
+ **max_bytes_for_level_multiplier_additional**：一个vector值，控制每层与其上一层的最大容量比例关系。vector中的每个值max_bytes_for_level_multiplier即是实际的比例关系。
+ **max_bytes_for_level_base**：Level 1可以存储的最大数据容量。建议与Level 0的容量大小一样。
+ **target_file_size_base**：Level 1的sst文件大小。建议是max_bytes_for_level_base / 10，即Level 1有10个sst文件。
+ **target_file_size_multiplier**：Level N+1 的sst文件大小是Level N的 sst文件大小多少倍。默认是1，即Level 1到Level N的sst文件大小是一样的。
+ **min_partial_merge_operands**：
+ **table_factory**：
+ **max_successive_merges**：
+ **arena_block_size**：
+ **merge_operator**：
+ **num_levels**：RocksDB的level层数，默认是7。设置更多的level层数，可以存储更多的数据，即使用不到那么多的level，对性能也无影响。
+ **write_buffer_size**： 一个MemTable的大小（MemTable存在内存中）。
+ **max_write_buffer_number**：最多可以有几个MemTable。默认是2。
+ **max_write_buffer_number_to_maintain**：MemTable被flush到磁盘后，最多可以在内存中保留几个MemTable。这些MemTable在启用TransactionDB时，用于检查写冲突。
+ **min_write_buffer_number_to_merge**：Memtable写入磁盘之前，最少需要几个Memtable先merge。
+ **prefix_extractor**：
+ **bloom_locality**：
+ **level0_file_num_compaction_trigger**： 当level0的文件数大于该值，会触发compaction。
+ **level0_slowdown_writes_trigger**：当level0的文件数大于该值，会明显slowdown写入速度。
+ **level0_stop_writes_trigger**： 当level0的文件数大于该值，会拒绝写入。
+ **memtable_huge_page_size**：
+ **max_compaction_bytes**：
+ **hard_pending_compaction_bytes_limit**：
+ **soft_pending_compaction_bytes_limit**：
+ **comparator**：
+ **verify_checksums_in_compaction**：
+ **memtable_insert_with_hint_prefix_extractor**：
+ **force_consistency_checks**：
+ **paranoid_file_checks**：
+ **optimize_filters_for_hits**：
+ **level_compaction_dynamic_level_bytes**：
+ **purge_redundant_kvs_while_flush**：
+ **inplace_update_support**：
+ **compaction_style**：
+ **compaction_filter**：
+ **disable_auto_compactions**：是否关闭自动compaction。
+ **inplace_update_num_locks**：
+ **memtable_prefix_bloom_size_ratio**：
+ **report_bg_io_stats**：

## TableOptions

+ **read_amp_bytes_per_bit**：
+ **skip_table_builder_flush**：
+ **filter_policy**：
+ **verify_compression**：
+ **block_restart_interval**：
+ **block_size_deviation**：
+ **format_version**：
+ **index_block_restart_interval**：
+ **block_size**：
+ **whole_key_filtering**：
+ **no_block_cache**：
+ **checksum**：
+ **hash_index_allow_collision**：
+ **index_type**：
+ **pin_l0_filter_and_index_blocks_in_cache**：
+ **cache_index_and_filter_blocks_with_high_priority**：
+ **cache_index_and_filter_blocks**：
+ **flush_block_policy_factory**：