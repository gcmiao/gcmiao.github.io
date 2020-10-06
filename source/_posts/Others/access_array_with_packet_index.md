---
title: Access enoki::DynamicArray with Packet Index
date: 2020/10/6 21:52:29
cover: https://raw.githubusercontent.com/mitsuba-renderer/enoki/master/docs/enoki-logo.png
categories:
- Others
tags:
- enoki
---

In `enoki` library v0.1.0, the `operator []` of the `DynamicArray<Packet<...>>`, which stores `Packet` data, only accepts integer argument. Here is an example:
```
constexpr size_t PacketSize = enoki::max_packet_size / sizeof(float); // = 8
using FloatP   = Packet<float, PacketSize>;
using FloatX   = DynamicArray<FloatP>;

FloatX data(-1.f, 1.f, 2.f, 3.f, 4.f, 5.f, 6.f, 7.f, 8.f, 9.f, 10.f);
```
Of course we can access the tenth element by `data[9]`. Also, we can access it using `enoki::slice(data, 9)` because it is accually the tenth slice in the dynamic array. Besides, since the `PacketSize` is 8, the tenth element is located in the second place of the second packet. We can access it with `enoki::packet(data, 1)[1]` as well.

But what if we want to access `data` using an index that is encapsulated in struct `Packet`?
```
using UInt32P = Packet<uint32_t, PacketSize>;
UInt32P index;
FloatP data_i = data[index]; // Wrong
FloatP data_i = enoki::gather(data, index); // Right
```
We cannot access it using `data[index]` because there is no overload of `operator []` for struct `Packet`. In this case, we should use `enoki::gather` instead and get the result in `FloatP` which has the same size and the value type with `index` and `data` respectively. According to the [official document](https://enoki.readthedocs.io/en/master/reference.html#memory-operations), it is equivalent to the following scalar loop.
```
FloatP result;
for (size_t i = 0; i < index.size(); ++i)
    result[i] = ((float *) data.data())[index[i]];
```
We can get better performance with `enoki::gather` if the target hardware supports the specific instructions.

More information: [Reference of enoki](https://enoki.readthedocs.io/en/master/reference.html#memory-operations)
