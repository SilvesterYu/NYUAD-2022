# Steps to make the Quantum Stuff Work

1. Open NYUAD-2022 folder **from Desktop**, not the original one.
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
