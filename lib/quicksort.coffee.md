# Quicksort

simple quicksort implementation

	@quicksort = (list, first, last) ->
		first = 0 unless first?
		last = list.length-1 unless last?
		if first < last
			length = first + last
			half = Math.floor length / 2
			x = list[half]
			i = first
			j = last

			until i > j
				i++ while list[i] < x
				j-- while x < list[j]
				if i<= j
					temp = list[i]
					list[i] = list[j]
					list[j] = temp
					i++
					j--

			quicksort list, first, j
			quicksort list, i, last

