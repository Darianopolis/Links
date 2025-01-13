# Structured guides

### [Site - vkguide.dev](<https://vkguide.dev/>)

A well structured introduction to modern Vulkan concepts.

- `+` Uses features like `dynamic rendering`, `buffer device address`, and `push constants` to simplify API usage.
- `+/-` Abstracts away initialization using the `vkbootstrap` library.
    - Convenient, but also means it doesn't cover the underlying initialization required.

### [Site - vulkan-tutorial.com](https://vulkan-tutorial.com/)

A useful resource for covering initialization topics skipped by `vkguide`

- `+` Covers initialization manually
- `-` Handles separate presentation queue.
    - While technically valid, no actual hardware is capable of present but *not* available on a graphics/compute capable queue. So this is largely just a complication up front that isn't worth thinking about while getting started.

# Synchronization

Synchronization is easily one of the most complex and misunderstood aspects of Vulkan, and is crucial for writing correct and portable Vulkan applications.

### [Blog - Yet another blog explaining Vulkan synchronization](<http://themaister.net/blog/2019/08/14/yet-another-blog-explaining-vulkan-synchronization/>)

"Mandatory" reading for understanding synchronization in Vulkan

- `+` Great introduction to synchronization
- `-` Doesn't cover [VK_KHR_synchronization2](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_KHR_synchronization2.html>), a revision to many synchronization related operations.

## Synchronization 2

- [VK_KHR_synchronization2](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_KHR_synchronization2.html>) - core in 1.3

Revisions to many synchronization related operations.
- `+` Provides more fine grained stages
- `+` Provides more extensible versions of several synchronization operations

### [Blog - Understanding Vulkan Synchronization](<https://www.khronos.org/blog/understanding-vulkan-synchronization>)

- `+` Covers differences between [VK_KHR_synchronization2](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_KHR_synchronization2.html>) and old synchronization.

## Timeline semaphores

- [VK_KHR_timeline_semaphore](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_KHR_timeline_semaphore.html>) - core in 1.2

A new type of synchronization primitive that enables more powerful synchronization across both the device and host.

- `+` Uses a monotonically (always) increasing 64 bit "payload" value.
- `+` Specific payload values can be waited on any number of times
- `+` Can be signalled and waited on the host - Effectively renders `VkFence` obsolete for many operations.
- `-` Cannot be used with WSI (Windowing System Interface) commands such as `vkAcquireNextImageKHR` and `vkQueuePresent` directly.
    - However, you can "adapt" to/from binary semaphores in queue submissions with some limitations. (TODO)
- `-` Synchronization Validation is still not compatible with timeline semaphores

### [Blog - Vulkan Timeline Semaphores](<https://www.khronos.org/blog/vulkan-timeline-semaphores>)

# Other Useful Extensions

A collection of widely supported extensions (many of which are core) to simplify and empower your usage of Vulkan.

## Dynamic Rendering

- [VK_KHR_dynamic_rendering](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_KHR_dynamic_rendering.html>) - core in 1.2

A new dynamic API for beginning renderpasses on demand.

-  `+` Render passes can be started while recording with no previous set up `vkCmdBeginRendering` - No more `VkRenderPass` or `VkFramebuffer` objects to manage.
- `+` Pipelines don't have to be renderpass aware anymore - They only need to know the set of formats they will be rendering to.
- `+` No performance loss on conventional desktop hardware.
- `+` More closely models DirectX's default rendering process.
- `-` Requires additional extensions ([VK_KHR_dynamic_rendering_local_read](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_KHR_dynamic_rendering_local_read.html>)) to take advantage of tiled GPU architectures (typically found in low power mobile/embedded devices) as can be achieved via subpass input attachments.

### [Blog - VK_KHR_dynamic_rendering tutorial](<https://lesleylai.info/en/vk-khr-dynamic-rendering/>)

## Buffer Device Addresses

- [VK_KHR_buffer_device_address](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_KHR_buffer_device_address.html>) - core in 1.3

Operations to acquire and use buffer addresses directly, bypassing the need for many buffer descriptors.

- `+` Extremely flexible freeform buffer addressing in shaders, including nested buffer references.
- `+` No descriptor management - Less overhead configuring and switching out addresses (can simply be passed around in push constants or other buffers)
- `-` No robustness or bounds validation - Buffer addresses don't track the accessible range of a buffer, as with raw pointers in host code there are no safety barriers here.

### [Documentation - Buffer device address](<https://docs.vulkan.org/samples/latest/samples/extensions/buffer_device_address/README.html>)

## Extended Dynamic State 1, 2, 3

- [VK_EXT_dynamic_state1](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_EXT_extended_dynamic_state.html>) - Core in 1.3
- [VK_EXT_dynamic_state2](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_EXT_extended_dynamic_state2.html>) - Partially core in 1.3
- [VK_EXT_dynamic_state3](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_EXT_extended_dynamic_state3.html>)

Additional dynamic states.

- `+` Lets you substantially reduce the amount of pipeline options you have to set up front when compiling pipelines
- `-` Some dynamic state operations come at the cost of shader performance - Which options and how much depends on the hardware and drivers being used.

## Maintenance Extensions

- [VK_KHR_maintenance1](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_KHR_maintenance1.html>) - core in 1.1
- [VK_KHR_maintenance2](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_KHR_maintenance2.html>) - core in 1.1
- [VK_KHR_maintenance3](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_KHR_maintenance3.html>) - core in 1.1
- [VK_KHR_maintenance4](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_KHR_maintenance4.html>) - core in 1.3
- [VK_KHR_maintenance5](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_KHR_maintenance5.html>)
- [VK_KHR_maintenance6](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_KHR_maintenance6.html>)
- [VK_KHR_maintenance7](<https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_KHR_maintenance7.html>)

A collection of API additions and revisions that didn't warrant their own extensions. Notably including some of the following

- Maintenance 5 deprecates `VkShaderModule`. Instead pass `VkShaderModuleCreateInfo` to the `pNext` of a `VkShaderStageCreateInfo` during pipeline creation - Eliminating the management of `VkShaderModule` objects in favor of managing the SPIR-V bytecode itself directly.

## Graphics Pipeline Libraries

# Useful Libraries

## Vulkan Memory Allocator

## Volk

## vk-bootstrap
