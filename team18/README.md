# qVerified - Verified Quantum Circuit Compilation
## NYUAD Hackathon for social good in the arab world

### Team

 - Lukas Burgholzer (Mentor), Johannes Kepler University Linz, Austria
 - Abdelkhalik Aljuneidi (Mentor), Takalam L.T.D., Palestine
 - Geon Tack Lee (Hacker), New York University Abu Dhabi, UAE
 - Karim Wen Rahme (Hacker), New York University Abu Dhabi, UAE
 - Gayatri Tyagi (Hacker), New York University Tandon, UAE
 - Elijah Whittle (Hacker), New York University Tandon, UAE
 - Omar AlRemeithi (Hacker), Khalifa University, UAE
 - Silvey Yu (Hacker), New York University Shanghai, China
 - Wen Rahme (Hacker), New York University Abu Dhabi, UAE
 - Iheb Nassim Aouadj (Hacker), Higher National School of Computer Science Algiers, Algeria

### Getting started

Clone this repository
```console 
git clone https://github.com/burgholzer/NYUAD-2022
```
Our team's contribution is in the `team18` folder. So move there
```console
cd NYUAD-2022/team18
```
A minimum working example showcasing our contributions can be found in the `main.py` file.
To get this working, create and activate a new virtual environment for the project:
```console
python3 -m venv venv
. venv/bin/activate
```
Install the requirements from the `requirements.txt` file:
```console 
pip install -r requirements.txt
```
Then execute the Python script
```console 
python3 main.py
```
This should yield:
```console
Generating profile for verification ...
... generated profile
Generating application circuit ...
... generated application circuit
Compiling and verifying the circuit ...
... compiled circuit is equivalent to original circuit: True
Simulating the resulting circuit and saving histogram ...
... done
Compiling and verifying the circuit and introducing an error ...
... compiled circuit is equivalent to original circuit: False
Simulating the resulting circuit and saving histogram ...
... done
```

## Detailed list of contributions
We developed an easy-to-use web tool that allows to verify that quantum circuits have been correctly compiled.
To this end, we
 - Improved the quantum circuit equivalence checking tool MQT QCEC by developing a dedicated profile for verifying compilation results (see `generate_profile.py`)
 - Implemented a flow for using the generated profile to automatically verify the compilation of a circuit (see `backend/app/compile_and_verify`)
 - Designed a web application for easily using the developed methods, see the screenshot below

![demo](images/demo.png)

## Technologies used
 - IBM Qiskit: Quantum SDK
 - MQT QCEC: A tool for quantum circuit equivalence checking
 - Flask: Backend for the web application
 - React: Frontend for the web application

# Windows Instructions - Steps to make it Work

1. Clone this repo to Desltop. Open NYUAD-2022 folder **from Desktop**.
2. Go in team18
3. Open frontend in new window
4. Open backend in new window
5. For both frontend and backend, right click on the explorer and open in terminal.
6. In frontend terminal, go

```
npm run dev
```

7. Before you run wsgi, go to your C:\Users\yulif\Desktop\NYUAD-2022\team18\backend\app folder. If you find code.py, delete it

8. In the backend terminal, go

```
python3 wsgi.py
```

9. Go to your browser, and open [http://localhost:3000](http://localhost:3000)

10. Copy and paste the following code

```python
import os

from qiskit import *
from qiskit.circuit.library import PhaseOracle


def generate_circuit():
    filename = "3sat.dimacs"
    dimacs_specification = "c example DIMACS CNF 3-SAT\np cnf 3 3\n1 2 -3 0\n-1 2 -3 0\n-1 -2 -3 0\n"
    with open(filename, "wt") as file:
        file.write(dimacs_specification)
        file.close()

    oracle = PhaseOracle.from_dimacs_file(filename)
    os.remove(filename)
    oracle.name = "$U_\omega$"
    return oracle


def diffuser(nqubits):
    qc = QuantumCircuit(nqubits)
    for qubit in range(nqubits):
        qc.h(qubit)
    for qubit in range(nqubits):
        qc.x(qubit)
    qc.h(nqubits - 1)
    qc.mct(list(range(nqubits - 1)), nqubits - 1)
    qc.h(nqubits - 1)
    for qubit in range(nqubits):
        qc.x(qubit)
    for qubit in range(nqubits):
        qc.h(qubit)
    u_s = qc.to_gate()
    u_s.name = "$U_s$"
    return u_s
```

11. Magic!!
