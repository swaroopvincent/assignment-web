/**
 * Get the jobs from the jobs data base
 */

$(document).ready(function() {
    $.getJSON("data/jobs.json", function(data) {
        
        var jobs = [];
        $.each(data, function(index, value) {
            jobs.push(value)
        })

        var table = document.createElement("table")
        table.setAttribute("id", "myTable")
        var tableHeader = document.createElement("th")
        // tableHeader.innerHTML = "Current Jobs"
        var tableRow = table.insertRow(-1)
        tableRow.appendChild(tableHeader)
        for(let job of jobs) {
            tableRow = table.insertRow(-1)
            var tableCell = tableRow.insertCell(-1)
            var text = job['title'] + " - " + job['category']
            var aRef = document.createElement('a');
            var linkText = document.createTextNode(text)
            aRef.appendChild(linkText)
            aRef.title = text
            aRef.href = "job-details.html?jobId="+job['id']
            aRef.target = "_self"

            var pT = document.createElement('p')
            pT.innerHTML = job['company'] + "<br>" + job['location']
            var pTd = document.createElement('p')
            pTd.innerHTML = job['salary']  + "<br>" + job['details']

            tableCell.appendChild(aRef)
            tableCell.appendChild(pT)
            tableCell.appendChild(pTd)
        }

        var divContainer = document.getElementById("show-jobs")
        divContainer.innerHTML = "";
        divContainer.appendChild(table)

        $("#myInput").on("keyup", function() {
            var value = $(this).val().toLowerCase();
            $("#myTable tr").filter(function() {
              $(this).toggle($(this).text().toLowerCase().indexOf(value) > -1)
            });
          });
            
        

    }).fail(function() {
        console.log("error while retrieving the jobs from server")
    })
});
