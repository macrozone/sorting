# different Sorting algorithm
	
	if Meteor.isClient
		running = false
		Session.set "n", 1000
		$chart = {}

## shell-sort h-series

different h series for the shell-sorts

		h1 = (i, h) -> Math.pow 2, i
		h2 = (i, h) -> Math.pow(2, i)-1
		h3 = (i, h) -> 3*h + 1
	
	

## The plots



		plots = [
			{label: "quicksort", func:(n) => mesureTime n, @quicksort}
			{label: "shellsort", func:(n) => mesureTime n, @shellsort}
			{label: "shellsort h1", func:(n) => mesureTime n, (list) -> @shellsort_h list, h1}
			
			{label: "shellsort h2", func:(n) => mesureTime n, (list) -> @shellsort_h list, h2}
			{label: "shellsort h3", func:(n) => mesureTime n, (list) -> @shellsort_h list, h3}
		
to compare, we have a n * log(n) graph, factor is more or less ranom, it may differ on different systems

			{label: "c * n * log(n)", func: (n) => 0.00001*n*Math.log n}
			]
		
		
		mesureTime = (n, algo) ->
			data = @shuffle [0..n]
			startTime = new Date().getTime()
			algo data
			endTime = new Date().getTime()
			endTime - startTime
		
		reset = ->
			Session.set "n", 1000
			running = false
			chart = $chart.highcharts()
			while chart.series.length > 0 
				chart.series[0].remove true
		
		runTest = (n) ->

			chart = $chart.highcharts()
			for plot, index in plots
				
				chart.addSeries {name: plot.label, data:[]} unless chart.series[index]?

				t = plot.func n if chart.series[index].visible
				
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



		Template.tests.n = -> Math.round Session.get "n"

		Template.tests.events =
			"click .reset": reset
			"click .start": start

		Template.chart.rendered = ->
		
## highcharts config

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
				
			
		
