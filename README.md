# Hash::Util::Set and Hash::Util::Join

A collection of utilities for set operations and SQL-like joins on Perl hashes.

## Modules

### Hash::Util::Set - Set Operations on Hash Keys

Provides set operations on hash keys.

**Set Operations:**
- `keys_union` - Keys in either hash (A ∪ B)
- `keys_intersection` - Keys in both hashes (A ∩ B)
- `keys_difference` - Keys in first but not second (A \ B)
- `keys_symmetric_difference` - Keys in either but not both (A △ B)
- `keys_partition` - Split into three sets: only A, both, only B

**Set Predicates:**
- `keys_disjoint` - No common keys?
- `keys_equal` - Same keys?
- `keys_subset` - A ⊆ B?
- `keys_proper_subset` - A ⊂ B (strict)?
- `keys_superset` - A ⊇ B?
- `keys_proper_superset` - A ⊃ B (strict)?

**Membership Tests:**
- `keys_any` - At least one key exists?
- `keys_all` - All keys exist?
- `keys_none` - No keys exist?

**Example:**
```perl
%x = (a => 1, b => 2, c => 3);
%y = (b => 4, c => 5, d => 6);

@keys = keys_union %x, %y;                 # (a, b, c, d)
@keys = keys_intersection %x, %y;          # (b, c)
@keys = keys_difference %x, %y;            # (a)
@keys = keys_symmetric_difference %x, %y;  # (a, d)

$bool = keys_disjoint %x, %y;              # false
$bool = keys_subset %x, %y;                # false
$bool = keys_any %x, 'a', 'z';             # true
```

### Hash::Util::Join - SQL-like Join Operations

Provides SQL-like join operations for combining two hashes based on their keys.

**Join Operations:**
- `hash_inner_join` - Keys in both hashes
- `hash_left_join` - All keys from left hash
- `hash_right_join` - All keys from right hash
- `hash_outer_join` - All keys from both hashes
- `hash_left_anti_join` - Keys only in left hash
- `hash_right_anti_join` - Keys only in right hash
- `hash_full_anti_join` - Keys in either but not both

**Example:**
```perl
%users  = (1 => 'Alice', 2 => 'Bob', 3 => 'Charlie'        );
%scores = (              2 => 95,    3 => 87,       4 => 92);

# Inner join - only matching keys
%result = hash_inner_join %users, %scores, sub {
  my ($key, $name, $score) = @_;
  "$name: $score";
};
# Result: (2 => 'Bob: 95', 3 => 'Charlie: 87')

# Left join - all users, with scores if available
%result = hash_left_join %users, %scores, sub {
  my ($key, $name, $score) = @_;
  defined $score ? "$name: $score" : "$name: no score";
};
# Result: (1 => 'Alice: no score', 2 => 'Bob: 95', 3 => 'Charlie: 87')

# Anti join - users without scores
%result = hash_left_anti_join %users, %scores;
# Result: (1 => 'Alice')
```

## Installation

```bash
cpanm Hash::Util::Set
```

Or manually:

```bash
perl Makefile.PL
make
make test
make install
```

## Performance

Both modules automatically use XS implementations when available, falling back to pure Perl otherwise for maximum compatibility and performance.

## Export Tags

Both modules support flexible importing:

```perl
use Hash::Util::Set qw(:all);             # All functions
use Hash::Util::Set qw(:operations);      # Set operations only
use Hash::Util::Set qw(:predicates);      # Predicates only
use Hash::Util::Set qw(:membership);      # Membership tests only

use Hash::Util::Join qw(:all);            # All join functions
use Hash::Util::Join qw(hash_inner_join); # Specific functions
```

## See Also

- [Hash::Util](https://metacpan.org/pod/Hash::Util) - Core Perl hash utilities
- [List::Util](https://metacpan.org/pod/List::Util) - List utilities
- [Set::Scalar](https://metacpan.org/pod/Set::Scalar) - Full-featured set operations
- [Set::Object](https://metacpan.org/pod/Set::Object) - Object-oriented sets

## Author

Christian Hansen <chansen@cpan.org>

## Copyright and License

Copyright (C) 2026 Christian Hansen

This library is free software; you can redistribute it and/or modify it under the same terms as Perl itself.
