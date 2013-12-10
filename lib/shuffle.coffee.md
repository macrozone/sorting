a shuffle function

    @shuffle = (a) ->
      # From the end of the list to the beginning, pick element `i`.
      for i in [a.length-1..1]
        # Choose random element `j` to the front of `i` to swap with.
        j = Math.floor Math.random() * (i + 1)
        # Swap `j` with `i`, using destructured assignment
        [a[i], a[j]] = [a[j], a[i]]
      # Return the shuffled array.
      a
