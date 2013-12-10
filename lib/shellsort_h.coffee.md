# Shellsort with custom h-series

a shellsort algorith that can take a h_series. the h_series should start with a h that is minor than n

	@shellsort_h = (a, h_series, first, last) ->
		first = 0 unless first?
		last = a.length-1 unless last?
		
		n = last - first + 1
		h = 1
		k = 0

		for h in h_series
			for i in [first..last-h]
				e = a[i+h]
				j = i
				while j >= first and e < a[j]
					a[j+h] = a[j]
					j = j-h
				a[j + h] = e
			
			


