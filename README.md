<!DOCTYPE html><html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Malla Curricular Veterinaria</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #f4f6f8;
      margin: 0;
      padding: 20px;
    }
    h1 {
      text-align: center;
      color: #2c3e50;
    }
    .grid {
      display: flex;
      gap: 20px;
      overflow-x: auto;
      padding: 10px;
    }
    .cycle {
      flex: 1;
      min-width: 220px;
      background: #ffffff;
      border-radius: 10px;
      padding: 10px;
      box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
    }
    .cycle h2 {
      font-size: 1.2em;
      text-align: center;
      color: #34495e;
    }
    .course {
      margin: 10px 0;
      padding: 10px;
      background: #dff1f1;
      border-radius: 6px;
      cursor: pointer;
      position: relative;
    }
    .course.completed {
      background: #a0d8b3;
      text-decoration: line-through;
    }
    .course.locked {
      background: #e0e0e0;
      color: #999;
      cursor: not-allowed;
    }
    .course.locked::after {
      content: attr(data-tooltip);
      position: absolute;
      background: #333;
      color: #fff;
      padding: 5px;
      border-radius: 5px;
      font-size: 0.8em;
      top: 50%;
      left: 105%;
      transform: translateY(-50%);
      white-space: nowrap;
      display: none;
      z-index: 1;
    }
    .course.locked:hover::after {
      display: block;
    }
  </style>
</head>
<body>
  <h1>Malla Curricular - Medicina Veterinaria</h1>
  <div class="grid" id="curriculum"></div>
  <script>
    const curriculumData = {
      "I": [
        { id: "1", name: "INTRODUCCIÓN A LA MEDICINA VETERINARIA Y ZOOTECNIA" },
        { id: "2", name: "QUÍMICA GENERAL" },
        { id: "3", name: "BIOLOGÍA GENERAL" },
        { id: "4", name: "MATEMÁTICA" },
        { id: "5", name: "METODOLOGÍA DEL APRENDIZAJE UNIVERSITARIO" },
        { id: "6", name: "COMUNICACIÓN I" },
        { id: "7", name: "RECURSOS DIGITALES" }
      ],
      "II": [
        { id: "8", name: "COMUNICACIÓN II", requires: ["6"] },
        { id: "9", name: "FILOSOFÍA DEL PENSAMIENTO CRÍTICO" },
        { id: "10", name: "BIOESTADÍSTICA", requires: ["4"] },
        { id: "11", name: "QUÍMICA ORGÁNICA", requires: ["2"] },
        { id: "12", name: "BIOFÍSICA" },
        { id: "13", name: "ANATOMÍA ANIMAL I", requires: ["3"] }
      ],
      "III": [
        { id: "14", name: "ANATOMÍA ANIMAL II", requires: ["13"] },
        { id: "15", name: "EMBRIOLOGÍA E HISTOLOGÍA VETERINARIA", requires: ["13"] },
        { id: "16", name: "GENÉTICA ANIMAL", requires: ["3"] },
        { id: "17", name: "MÉTODOS ESTADÍSTICOS", requires: ["10"] },
        { id: "18", name: "BIOQUÍMICA", requires: ["2"] },
        { id: "19", name: "REALIDAD NACIONAL Y GLOBAL" }
      ],
      "IV": [
        { id: "20", name: "FISIOLOGÍA ANIMAL", requires: ["18"] },
        { id: "21", name: "HEMATOLOGÍA E INMUNOLOGÍA VETERINARIA", requires: ["15"] },
        { id: "22", name: "MEJORAMIENTO ANIMAL", requires: ["16"] },
        { id: "23", name: "MICROBIOLOGÍA VETERINARIA", requires: ["17"] },
        { id: "24", name: "CULTIVO Y MANEJO DE FORRAJES", requires: ["1"] },
        { id: "25", name: "ÉTICA, CIUDADANÍA E INCLUSIÓN SOCIAL" }
      ],
      "V": [
        { id: "26", name: "FARMACOLOGÍA Y TOXICOLOGÍA VETERINARIA", requires: ["20"] },
        { id: "27", name: "SEMIOLOGÍA VETERINARIA", requires: ["20"] },
        { id: "28", name: "PARASITOLOGÍA Y ENFERMEDADES PARASITARIAS", requires: ["14"] },
        { id: "29", name: "PATOLOGÍA VETERINARIA", requires: ["20"] },
        { id: "30", name: "REPRODUCCIÓN ANIMAL Y TECNOLOGÍAS REPROD.", requires: ["15", "20"] },
        { id: "31", name: "NUTRICIÓN ANIMAL", requires: ["18", "20"] },
        { id: "32", name: "MEDIO AMBIENTE Y DESARROLLO SOSTENIBLE" }
      ],
      "VI": [
        { id: "33", name: "IMAGENOLOGÍA", requires: ["13"] },
        { id: "34", name: "ALIMENTACIÓN ANIMAL", requires: ["24"] },
        { id: "35", name: "LABORATORIO CLÍNICO VETERINARIO", requires: ["23", "27", "29"] },
        { id: "36", name: "ENFERMEDADES INFECCIOSAS", requires: ["23", "27"] },
        { id: "37", name: "FISIOPATOLOGÍA VETERINARIA", requires: ["27", "29"] },
        { id: "38", name: "METODOLOGÍA DE LA INVESTIGACIÓN CIENTÍFICA" },
        { id: "39", name: "TALLER DE INNOVACIÓN Y EMPRENDIMIENTO" }
      ],
      "VII": [
        { id: "40", name: "INSPECCIÓN SANITARIA DE PRODUCTOS PECUARIOS", requires: ["28"] },
        { id: "41", name: "SALUD PÚBLICA VETERINARIA", requires: ["28", "32"] },
        { id: "42", name: "TECNOLOGÍA DE CARNE Y LECHE", requires: ["34"] },
        { id: "43", name: "EPIDEMIOLOGÍA VETERINARIA", requires: ["28"] },
        { id: "44", name: "ELECTIVO 1" },
        { id: "45", name: "ELECTIVO 2" },
        { id: "46", name: "ETOLOGÍA APLICADA Y BIENESTAR ANIMAL", requires: ["27"] }
      ],
      "VIII": [
        { id: "47", name: "ELECTIVO 3" },
        { id: "48", name: "CIRUGÍA VETERINARIA EN ANIMALES MENORES", requires: ["33"] },
        { id: "49", name: "PRODUCCIÓN Y SANIDAD DE PORCINOS", requires: ["34", "41"] },
        { id: "50", name: "MEDICINA VETERINARIA EN ANIMALES DE COMPAÑÍA", requires: ["35", "43"] },
        { id: "51", name: "GESTIÓN DE NEGOCIOS VETERINARIOS Y PECUARIOS", requires: ["39", "41"] },
        { id: "52", name: "DEONTOLOGÍA PROFESIONAL", requires: ["46"] }
      ],
      "IX": [
        { id: "53", name: "ELECTIVO 4" },
        { id: "54", name: "MEDICINA VETERINARIA EN ANIMALES MAYORES", requires: ["50"] },
        { id: "55", name: "PRODUCCIÓN Y SANIDAD DE AVES", requires: ["34", "47"] },
        { id: "56", name: "PRODUCCIÓN Y SANIDAD DE VACUNOS DE LECHE", requires: ["22", "34", "51"] },
        { id: "57", name: "CIRUGÍA VETERINARIA EN ANIMALES MAYORES", requires: ["48"] },
        { id: "58", name: "TESIS I", requires: ["63"] },
        { id: "59", name: "ELECTIVO 5" }
      ],
      "X": [
        { id: "60", name: "TESIS II", requires: ["58"] },
        { id: "61", name: "PRÁCTICAS PREPROFESIONALES", requires: ["54", "55", "56", "57"] }
      ]
    };const state = JSON.parse(localStorage.getItem("vetCurriculumState") || "{}");

const saveState = () => {
  localStorage.setItem("vetCurriculumState", JSON.stringify(state));
};

const isUnlocked = (course) => {
  if (!course.requires) return true;
  return course.requires.every(req => state[req]);
};

const render = () => {
  const container = document.getElementById("curriculum");
  container.innerHTML = "";

  for (const [cycle, courses] of Object.entries(curriculumData)) {
    const column = document.createElement("div");
    column.className = "cycle";
    column.innerHTML = `<h2>Ciclo ${cycle}</h2>`;

    courses.forEach(course => {
      const div = document.createElement("div");
      const unlocked = isUnlocked(course);
      const completed = state[course.id];

      div.className = `course ${completed ? "completed" : ""} ${!unlocked ? "locked" : ""}`;
      div.textContent = course.name;
      if (!unlocked) {
        const pending = course.requires.filter(r => !state[r]);
        div.setAttribute("data-tooltip", `Requiere: ${pending.join(", ")}`);
      }
      if (unlocked) {
        div.onclick = () => {
          state[course.id] = !state[course.id];
          saveState();
          render();
        };
      }
      column.appendChild(div);
    });
    container.appendChild(column);
  }
};

render();

  </script>
</body>
</html>
