<!DOCTYPE html>
<html>
    <head>
        <title>My Medication Tracker</title>
    </head>

    <script>
        var scheduleData = [];

        function addToTable() {

            // get table values from  input fields
            var table = document.getElementById("scheduleTable");
            var compartment = document.getElementById("compartment").value;
            var date = document.getElementById("date").value;
            var time = document.getElementById("time").value;
            var medication = document.getElementById("medication").value;

            // check if the user has entered all fields
            if (compartment == "" || date == "" || time == "") {
                alert("Please fill in all fields before adding to your schedule.");
                return;
            }

            // sort the table by compartment number using an array
            scheduleData.push({compartment: parseInt(compartment), date: date, time: time, medication: medication});
            scheduleData.sort(function(a, b) {
                return a.compartment - b.compartment;
            });
            updateTable();
        }

        function updateTable() {
            var table = document.getElementById("scheduleTable");
            table.innerHTML = `
                <tr>
                    <th> Compartment Number </th>
                    <th> Date </th>
                    <th> Time </th>
                    <th> Medication Name </th>
                </tr>`;

            scheduleData.forEach(entry => {
                var row = table.insertRow(-1);
                row.insertCell(0).innerHTML = entry.compartment;
                row.insertCell(1).innerHTML = entry.date;
                row.insertCell(2).innerHTML = entry.time;
                row.insertCell(3).innerHTML = entry.medication ? entry.medication : "N/A";
            });
        }

        function sendData() {
            scheduleData.sort(function(a, b) {
                return a.compartment - b.compartment;
            });

            fetch("http://localhost:5000/sendData", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify(scheduleData)
            }).then(response => response.text()).then(data =>{
                alert("Schedule successfully sent! : " + data);
            }).catch(error => {
                alert("Error sending your schedule! Is your Python server connected?");
            });
        }
    </script>
    <body>
        <h1> My Medication Tracker</h1>
        <h2> Welcome to My Medication Tracker! </h2>
        <p> My Medication Tracker is a web application desgined for users to track their medication schedule, 
            while using <i> super cool name </i>! You can easily add, update, and delete medications from 
            your schedule for up to <b> 14 dosages </b>. </p>

        <h2> Enter Your Medication Schedule Here! </h2>
            <p> As you fill in your medication into each compartment <i> super cool name </i>, please enter \
                the date and time you would like to be reminded to take the medication at. <i> Each compartment 
                has a small number at the bottom, which represents the corresponding compartment number. </i> </p>

                <label> Compartment Number (1-14) : </label>
                <input type = "number" id="compartment" name="compartment number" min = "0" max = "14" required> <br>

                <label> Date : </label>
                <input type = "date" id="date" name="date" required> <br>

                <label> Time : </label>
                <input type = "time" id="time" name="time" required> <br>

                <label> Medication Name <i> (if applicable) </i> : </label>
                <input type = "text" id="medication" name="medication"> <br>

                <button type = "button" id="add" onclick = "addToTable()"> Add Medication to your Schedule </button>

        <h2> Your Medication Schedule </h2>
            <p> Below is your medication schedule. Please ensure all details are correct before clicking submit. </p>

            <table border = "1" id ="scheduleTable">
                <tr>
                    <th> Compartment Number </th>
                    <th> Date </th>
                    <th> Time </th>
                    <th> Medication Name </th>
                </tr>
            </table>
    </body>
