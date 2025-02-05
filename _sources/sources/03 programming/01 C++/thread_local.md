# thread_local

* [cppreference - Static_block_variables](https://en.cppreference.com/w/cpp/language/storage_duration#Static_block_variables)
  * Block variables with static or thread(since C++11) storage duration are initialized the first time control passes through their declaration (unless their initialization is zero- or constant-initialization, which can be performed before the block is first entered). On all further calls, the declaration is skipped.
