# different Sorting algorithm
	
	if Meteor.isClient
		running = false
		Session.set "n", 1000
		$chart = {}

## shell-sort h-series

different h series for the shell-sorts



		h_shell = (i, h) -> Math.pow 2, i
		h_hibbard = (i, h) -> Math.pow(2, i)-1
		h_knuth = (i, h) -> 3*h + 1
	
	
		make_h_serie = (n, h_func) ->
			h_serie = [1]
			i = 0
			h = 1
			while h < n
				h_serie.push h unless h <=1
				h = h_func(i,h)
				i++
			return h_serie.reverse()


## The plots



		plots = [
			{label: "quicksort", visibleAtStartup: true, func:(n) => mesureTime n, @quicksort}
			{label: "shellsort", visibleAtStartup: true, func:(n) => mesureTime n, @shellsort}
			{label: "shellsort h_shell (slow!)", visibleAtStartup: false, func:(n) => 
				h_serie = make_h_serie n, h_shell
				mesureTime n, (list) -> @shellsort_h list, h_serie}
			{label: "shellsort h_hibbard (slow!)", visibleAtStartup: false, func:(n) => 
				h_serie = make_h_serie n, h_hibbard
				mesureTime n, (list) -> @shellsort_h list, h_serie}
			{label: "shellsort h_knuth (slow!)", visibleAtStartup: false, func:(n) => 
				h_serie = make_h_serie n, h_knuth
				mesureTime n, (list) -> @shellsort_h list, h_serie}
			
to compare, we have a n * log(n) graph, factor is more or less ranom, it may differ on different systems

			{label: "c * n * log(n)", visibleAtStartup: true, func: (n) => 0.00001*n*Math.log n}
			]
		
		
		mesureTime = (n, algo) ->
			data = @shuffle [0..n]
			startTime = new Date().getTime()
			algo data
			endTime = new Date().getTime()
			endTime - startTime
		
		
		runTest = (n) ->

			chart = $chart.highcharts()
			for plot, index in plots
				if chart.series[index].visible
					t = plot.func n
					chart.series[index].addPoint [n, t]
				
			
		
		start = ->
			if !running
				running = true
				run()
			else
				running = false


		run = ->
			if running
				runTest Math.round Session.get "n"
				Meteor.setTimeout ->
					if running
						n = Session.get "n"
						n *= 1.25
						Session.set "n", n
						run()
				,200



		reset = ->

			Session.set "n", 1000
			running = false
			chart = $chart.highcharts()

remove all series

			while chart.series.length > 0 
				chart.series[0].remove true

readd series

			for plot, index in plots
				chart.addSeries name: plot.label, data:[], visible: plot.visibleAtStartup

		Template.tests.n = -> Math.round Session.get "n"

		Template.tests.events =
			"click .reset": reset
			"click .start": start

## highcharts config

		initChart = ->
			$chart = $(this.find(".chart")).highcharts
				chart:
					height: 600
					zoomType: 'x'
				legend:
					layout: "vertical"
					itemStyle:
						paddingTop: "8px"
						paddingBottom: "8px"
				type: "bar"
				title: "Sortings"
				xAxis:
					type: "logarithmic"
					minorTickInterval: 0.1
					title:
						text: "n"
				yAxis:

					type: "logarithmic"
					title:
						text: "time taken in ms"

		Template.chart.rendered = ->

			initChart.apply this
			reset()
		






				
			
		
