
target_sources(foo
  PRIVATE
  foo.cc
)

b_embed(foo prayer.txt)
