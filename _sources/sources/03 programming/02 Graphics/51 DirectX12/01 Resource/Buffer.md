# Buffer

## Buffer Alignment
Buffer alignment restrictions:

* Linear subresource copying must be aligned to 512 bytes (with the row pitch aligned to D3D12_TEXTURE_DATA_PITCH_ALIGNMENT bytes).
* Constant data reads must be a multiple of 256 bytes from the beginning of the heap (i.e. only from addresses that are 256-byte aligned).
* Index data reads must be a multiple of the index data type size (i.e. only from addresses that are naturally aligned for the data).
* ID3D12GraphicsCommandList::ExecuteIndirect data must be from offsets that are multiples of 4 (i.e. only from addresses that are DWORD aligned).

> Reference  
> [learn.microsoft - upload-and-readback-of-texture-data#buffer-alignment](https://learn.microsoft.com/en-us/windows/win32/direct3d12/upload-and-readback-of-texture-data#buffer-alignment)  