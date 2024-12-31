<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Online Hospital Management</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f9;
            color: #333;
        }
        header {
            background-color: #007bff;
            color: white;
            padding: 1rem;
            text-align: center;
        }
        .container {
            padding: 2rem;
        }
        .console {
            background-color: #000;
            color: #0f0;
            padding: 1rem;
            border-radius: 5px;
            height: 300px;
            overflow-y: auto;
            font-family: monospace;
        }
        input[type="text"] {
            width: calc(100% - 120px);
            padding: 0.5rem;
            margin-right: 10px;
        }
        button {
            padding: 0.5rem 1rem;
            background-color: #007bff;
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <header>
        <h1>Online Hospital Management</h1>
    </header>
    <div class="container">
        <div class="console" id="consoleOutput"></div>
        <div style="margin-top: 1rem;">
            <input type="text" id="commandInput" placeholder="Enter your command here...">
            <button onclick="executeCommand()">Execute</button>
        </div>
    </div>

    <script>
        const consoleOutput = document.getElementById('consoleOutput');
        const commandInput = document.getElementById('commandInput');

        const hospitalData = {
            patients: [],
            doctors: [],
            appointments: []
        };

        function logToConsole(message) {
            consoleOutput.innerHTML += `> ${message}<br>`;
            consoleOutput.scrollTop = consoleOutput.scrollHeight;
        }

        function executeCommand() {
            const command = commandInput.value.trim();
            commandInput.value = '';

            if (!command) {
                logToConsole('Please enter a command.');
                return;
            }

            const [action, ...args] = command.split(' ');

            switch (action.toLowerCase()) {
                case 'addpatient':
                    addPatient(args);
                    break;
                case 'adddoctor':
                    addDoctor(args);
                    break;
                case 'listpatients':
                    listPatients();
                    break;
                case 'listdoctors':
                    listDoctors();
                    break;
                case 'bookappointment':
                    bookAppointment(args);
                    break;
                case 'help':
                    displayHelp();
                    break;
                default:
                    logToConsole('Unknown command. Type "help" for a list of commands.');
            }
        }

        function addPatient(args) {
            const [name, age] = args;
            if (!name || !age) {
                logToConsole('Usage: addpatient [name] [age]');
                return;
            }
            hospitalData.patients.push({ name, age });
            logToConsole(`Patient added: ${name}, Age: ${age}`);
        }

        function addDoctor(args) {
            const [name, specialization] = args;
            if (!name || !specialization) {
                logToConsole('Usage: adddoctor [name] [specialization]');
                return;
            }
            hospitalData.doctors.push({ name, specialization });
            logToConsole(`Doctor added: ${name}, Specialization: ${specialization}`);
        }

        function listPatients() {
            if (hospitalData.patients.length === 0) {
                logToConsole('No patients found.');
                return;
            }
            logToConsole('Patients:');
            hospitalData.patients.forEach((patient, index) => {
                logToConsole(`${index + 1}. ${patient.name}, Age: ${patient.age}`);
            });
        }

        function listDoctors() {
            if (hospitalData.doctors.length === 0) {
                logToConsole('No doctors found.');
                return;
            }
            logToConsole('Doctors:');
            hospitalData.doctors.forEach((doctor, index) => {
                logToConsole(`${index + 1}. Dr. ${doctor.name}, Specialization: ${doctor.specialization}`);
            });
        }

        function bookAppointment(args) {
            const [patientName, doctorName] = args;
            if (!patientName || !doctorName) {
                logToConsole('Usage: bookappointment [patientName] [doctorName]');
                return;
            }
            const patient = hospitalData.patients.find(p => p.name === patientName);
            const doctor = hospitalData.doctors.find(d => d.name === doctorName);

            if (!patient) {
                logToConsole(`Patient not found: ${patientName}`);
                return;
            }
            if (!doctor) {
                logToConsole(`Doctor not found: ${doctorName}`);
                return;
            }

            hospitalData.appointments.push({ patientName, doctorName });
            logToConsole(`Appointment booked: ${patientName} with Dr. ${doctorName}`);
        }

        function displayHelp() {
            logToConsole(`Available commands:
- addpatient [name] [age]
- adddoctor [name] [specialization]
- listpatients
- listdoctors
- bookappointment [patientName] [doctorName]
- help`);
        }
    </script>
</body>
</html>
