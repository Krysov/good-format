![good-format-logo](https://github.com/Krysov/good-format/assets/12496282/b1544619-716e-4b6f-a6aa-f8784bb7cc51)

# About
A versatile binary format aimed at both document files and service messages.

# Goals
- Be a versatile and feature rich binary format with size and reading speed in mind.
- Have future proof specifications and ingrained data integrity checking.
- Targeted at document and project files with structure optimization, data recoverability and in place editing. Possibly multi-threadable and streamable reading and writing.
- Data indexing and support for enormous data sized beyond 4GB.

# Non-Goals
- Be interoperable with JSON. If it works however, it's a coincidence not a given.

# Structure
By using _variable length qualifiers_, the size limit of 2^32-1 bytes or 4GB-1B of any data can be lifted while keeping size indicators of small values to a minimum.
Likewise VLQs are used for things like dictionary labels and the file's _variant version_ which is used as both a file's _magic number_ as well as the format identifier of the VLQs used in the given variant.
VLQs are also used for the type indicators, thus not limiting the number of possible types available. However, most will be mappable to a small collection of _primitive types_ of which most are defined on _page 1_.
Further pages for containing types like UUIDs and Files are work in progress and will be added at a later point.

The first element is a _variant version_. The idea is to have multiple formattings available geared either to size efficiency or data redundancy.
Followed by elements which consist of a Key-VLQ. If it is a container, a Size-VLQ or if it is an element contained by a dictionary, a Key-VLQ or key value. Lastly followed by the value, although in the case of Null, it is implied by the type indicator. 
_Markers_ are a special type whose elements can be placed between any other element without bumping the element count of any containers. These are intended as a versatile meta data point which introduces features like labels to edit data in place without needing to rewrite the entire file, a file index close to the end of the file or checkpoint markers which help to reconstruct the data structure if there has been some data corruption. Markers also reference a list of previous markers which aids in reconstruction of data or generating quick overviews of the contents.
Potentially one or more _portals_ which contain the payload of forward declared heavy data like huge binary sequences.
Lastly, a _backwards header_ which points to the nearest index or checkpoint marker to quickly gather an overview. This is an optional header and parsed in reverse since the second best place to begin reading a file is from the last byte.

# Page 1 Types
- Null
- Bool
- U8
- U16
- U32
- U64
- UDyn
- I8
- I16
- I32
- I64
- IDyn
- F8
- F16
- F32
- F64
- FDyn (WIP)
- Time (WIP)
- Key-VLQ (WIP)
- Size-VLQ (WIP)
- Type-VLQ (WIP)
- Octets
- String
- UTF8
- __Marker__
- __Portal__ (WIP)
- Fix Type List
- Dyn Type List
- VLQ-K/Fix-V Dict
- VLQ-K/Dyn-V Dict
- Fix-K/Fix-V Dict
- Fix-K/Dyn-V Dict

# Todos
- Implement a parser library in zig. (In Progress)
