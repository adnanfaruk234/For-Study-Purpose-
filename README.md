<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Subject Study Manager</title>

<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+Bengali&display=swap" rel="stylesheet">

<style>
body {
    font-family: 'Noto Sans Bengali', sans-serif;
    background: #f4f6f9;
    padding: 20px;
}

.container {
    max-width: 1100px;
    margin: auto;
    background: white;
    padding: 20px;
    border-radius: 10px;
    overflow-x: auto;
}

input, select, button {
    padding: 8px;
    margin-top: 5px;
}

table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 15px;
}

th, td {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: center;
    white-space: nowrap;
}

th {
    background: #4CAF50;
    color: white;
}
</style>
</head>

<body>

<div class="container">

<h2>Subject Wise Study Manager</h2>

<input type="text" id="newSubject" placeholder="নতুন সাবজেক্ট লিখুন">
<button onclick="addSubject()">Add Subject</button>

<select id="subjectSelect" onchange="loadTable()"></select>

<table>
<thead>
<tr>
<th>ক্রমিক নাম্বার</th>
<th>অধ্যায়ের নাম</th>
<th>কমপ্লিট</th>
<th>সোর্স</th>
<th>সিকিউ</th>
<th>এমসিকিউ</th>
</tr>
</thead>
<tbody id="tableBody"></tbody>
</table>

</div>

<script>

const defaultBiology = [
"কোষ ও এর গঠন",
"কোষ বিভাজন",
"কোষ রসায়ন",
"অণুজীব",
"শৈবাল ও ছত্রাক",
"ব্রাইওফাইটা ও টেরিডোফাইটা",
"নগ্নবীজী এবং আবৃতবীজী উদ্ভিদ",
"টিসু ও টিস্যুতন্ত্র",
"উদ্ভিদের শারীরতত্ত্ব",
"উদ্ভিদ প্রজনন",
"জীবপ্রযুক্তি",
"জীবের পরিবেশ, বিস্তার ও সংরক্ষণ"
];

function init() {
    if(!localStorage.getItem("subjects")) {
        const subjects = {
            "জীববিজ্ঞান প্রথম পত্র": defaultBiology.map((name, index) => ({
                serial: index + 1,
                name: name,
                complete: false,
                source: "",
                cq: "",
                mcq: ""
            }))
        };
        localStorage.setItem("subjects", JSON.stringify(subjects));
    }
    loadSubjects();
}

function loadSubjects() {
    const subjects = JSON.parse(localStorage.getItem("subjects"));
    const select = document.getElementById("subjectSelect");
    select.innerHTML = "";

    Object.keys(subjects).forEach(sub => {
        const option = document.createElement("option");
        option.value = sub;
        option.textContent = sub;
        select.appendChild(option);
    });

    loadTable();
}

function loadTable() {
    const subjects = JSON.parse(localStorage.getItem("subjects"));
    const current = document.getElementById("subjectSelect").value;
    const data = subjects[current];

    const tbody = document.getElementById("tableBody");
    tbody.innerHTML = "";

    data.forEach((row, index) => {
        const tr = document.createElement("tr");

        tr.innerHTML = `
        <td>${row.serial}</td>
        <td>${row.name}</td>
        <td><input type="checkbox" ${row.complete ? "checked" : ""} 
            onchange="updateField(${index}, 'complete', this.checked)"></td>
        <td><input value="${row.source}" 
            onchange="updateField(${index}, 'source', this.value)"></td>
        <td><input value="${row.cq}" 
            onchange="updateField(${index}, 'cq', this.value)"></td>
        <td><input value="${row.mcq}" 
            onchange="updateField(${index}, 'mcq', this.value)"></td>
        `;

        tbody.appendChild(tr);
    });
}

function updateField(index, field, value) {
    const subjects = JSON.parse(localStorage.getItem("subjects"));
    const current = document.getElementById("subjectSelect").value;
    subjects[current][index][field] = value;
    localStorage.setItem("subjects", JSON.stringify(subjects));
}

function addSubject() {
    const name = document.getElementById("newSubject").value;
    if(!name) return;

    const subjects = JSON.parse(localStorage.getItem("subjects"));
    subjects[name] = [];
    localStorage.setItem("subjects", JSON.stringify(subjects));
    document.getElementById("newSubject").value = "";
    loadSubjects();
}

init();

</script>

</body>
</html>
