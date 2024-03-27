<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.9.4/Chart.min.js"></script>
    <style>
        .container{
            background-color:#f3f4f7;
            width: 50%;
            margin: auto;
        }
    </style>
    <title>Custom graph</title>
</head>
<body>
    <h1>Thingspeak graph</h1>
    <div class="container">
        <canvas id="myChart"></canvas>
    </div>
    <p>View the live graph: <a href="https://yourusername.github.io/project1/project1.html" target="_blank">Graph Link</a></p>
    <script>
        $(document).ready(function() {
            function getData() {
                var url ="https://api.thingspeak.com/channels/2282916/fields/1,2,3.json?api_key=QN7LPRB8JPXVUF5N&results=2";

                $.getJSON(url, function(data) {
                    var field1Values = [];
                    var field2Values = [];
                    var field3Values = [];
                    var timestamps = [];

                    $.each(data.feeds, function(index, feed) {
                        field1Values.push(feed.field1);
                        field2Values.push(feed.field2);
                        field3Values.push(feed.field3);
                        timestamps.push(feed.created_at);
                    });
                    // Dealing with the graph
                    var ctx = document.getElementById('myChart').getContext('2d');

                    var chart = new Chart(ctx, {
                        type: 'bar',
                        data: {
                            labels: timestamps,
                            datasets: [{
                                label: 'Humidity',
                                data: field1Values,
                                backgroundColor: '#84bd00',
                                borderWidth: 1
                            },
                            {
                                label: 'Temperature',
                                data: field2Values,
                                backgroundColor: '#00205b',
                                borderWidth: 1
                            },
                            {
                                label: 'CO2',
                                data: field3Values,
                                backgroundColor: '#ff0000',
                                borderWidth: 1
                            }]
                        },
                        options: {
                            scales: {
                                yAxes: [{
                                    ticks: {
                                        beginAtZero: true
                                    }
                                }]
                            }
                        }
                    });
                });
            }

            getData(); // Initial call

            // Automatic refresh every 6.5 seconds
            setInterval(getData, 6500);
        });
    </script>
</body>
</html>
