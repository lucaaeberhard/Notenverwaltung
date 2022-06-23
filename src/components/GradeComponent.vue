<template>
  <div class="container">
    <h2 class="d-flex justify-content-center">Notenverwaltungstool</h2>

    <h3 class="d-flex justify-content-center mb-3 mt-3">Gesamtdurchschnitt: {{overallAverage}}</h3>

    <div class="input-group mb-5 w-50 mx-auto">
      <input type="text" v-model="newSubject" class="form-control" placeholder="New Subject">
      <div class="input-group-append">
        <button @click="addSubject(newSubject)" type="button" class="btn btn-primary">+</button>
      </div>
    </div>

    <div class="row ml-2">
      <ul class="col-md-3 list-group mb-5" v-for="subject in subjects">
        <li class="list-group-item active">{{subject.name}}</li>
        <li class="list-group-item">Durchschnitt: {{parseFloat(subject.average).toFixed(2)}}</li>
        <li class="list-group-item" v-for="(grades, index) in subject.grades">
          {{parseFloat(grades).toFixed(2)}}
          <button @click="deleteGrade(subject.name, index)" type="button" class="btn btn-danger m-3">X</button>
        </li>
        <li class="list-group-item">
          <div class="input-group">
            <input type="number" v-model="newGrade" class="form-control">
            <div class="input-group-append">
              <button @click="addGrade(newGrade, subject.name)" type="button" class="btn btn-primary">+</button>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
  
</template>

<script>
export default {
  name: 'GradeComponent',
  props: {
  },
  data: function() {
    return {
      subjects: [
        {name: "GES", grades: [5.00, 6.00], average: 0.00},
        {name: "M151", grades: [3.2, 5.5], average: 0.00},
        {name: "M152", grades: [4.5, 5.2], average: 0.00},
        {name: "M153", grades: [5.3, 4.00], average: 0.00},
        {name: "M306", grades: [3.5, 4.9], average: 0.00},
        {name: "NWS", grades: [5.00, 6.00], average: 0.00},
        {name: "SPK", grades: [5.00, 6.00], average: 0.00},
        {name: "SPO", grades: [5.00, 6.00], average: 0.00},
        {name: "WUR", grades: [5.00, 6.00], average: 0.00},
      ],
      newSubject: "",
      newGrade: "",
      overallAverage: ""
    }
  },
  methods: {
    addSubject: function(newSubject) {
      if(newSubject != "") {
        this.subjects.push({name: newSubject, grades: [], average: 0.00});
      }
    },
    addGrade: function(newGrade, subjectName) {
      if(newGrade.toString().split('.')[0].length == 1 && newGrade.toString().split('.')[0] <= 6 && newGrade.toString().split('.')[0] > 0) {
        this.subjects[this.subjects.findIndex((sub) => sub.name == subjectName)].grades.push(newGrade.toFixed(2));
        this.newGrade = "";
        this.updateAllAverages();
        this.updateOverAllAverage();
      }
    },
    deleteGrade: function(subjectName, index) {
      this.subjects[this.subjects.findIndex((sub) => sub.name == subjectName)].grades.splice(index, 1);
      this.updateAllAverages();
      this.updateOverAllAverage();
    },
    updateAllAverages: function() {
      this.subjects.forEach(subject => {
        let sum = 0.00;
        subject.grades.forEach(grade => {
          sum = parseFloat(sum.toString()) + parseFloat(grade.toString());
        })
        this.subjects[this.subjects.indexOf(subject)].average = sum / this.subjects[this.subjects.indexOf(subject)].grades.length;
      })
    },
    updateOverAllAverage: function() {
      let sum = 0.00;
      this.subjects.forEach(subject => {
        sum = parseFloat(sum.toString()) + parseFloat(subject.average.toString());
      })
      this.overallAverage = sum / this.subjects.length;
    }
  },
  beforeMount() {
    this.updateAllAverages();
    this.updateOverAllAverage();
  }
}
</script>


