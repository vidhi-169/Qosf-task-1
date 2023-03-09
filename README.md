# Qosf-task-1
You have two integers, either positive or negative, and the challenge is to generate a quantum algorithm that returns which is the larger number. Consider an appropriate number of qubits and explain why your proposal is valid for all kinds of numbers in case
1)
#CONVERT BIT STRING IN DECIMAL NUMBER
Suppose we have the number 1101=1*2^3 + 1*2^2 + 0*2^1 + 1*2^00
2)

NEED CORRESPONDING NUMBER OF QUBITS TO ENCODE AN ARBITARY NUMBER IN QUANTUM CIRCUIT
1 qubit encodes two numbers, 2 qubits encode 4 numbers and similarly n qubits encode 2^n numbers.
3)

COMPARISON
In order to compare two one digit number we only need a single qubit. Since we are comaring two numbers we need two qubits and an additional auxiliary qubit to store the result of comparison.

4)
There are total three possible outcomes:
outcome 1 = a>b (requires 1 aux qubit)
outcome 2 = b>a  (requires 1 aux qubit)
outcome 3 = a=b
the first auxiliary qubit is 1 if first number is greater than than second and the second aux qubit is 1 if second number is greater than first number
both aux qubits are  if numbers are equal

5)
a>b
Let q0 and q1 be the control qubits and the aux 0 be the target qubit. x gate would be applied on aux 0 iff q0 and q1 are |1>.If q0 and q1 =0 but the x gate flis the  >1 so, both q0=q1=1 now, so, mcx gate alies x gate on aux0. So aux0=1 iff(if and only if) q0=1 & q1=0


b>a
x gate would be applied on aux 1 iff bit 0=0 and bit 1=1. All the explanation same as above.

6)
Encoding the binary numbers into qubits. The encode function creates a single qubit circuit. It applies the x gate only if the secified o. is 1 and does nothing if its 0.(so this is the ste where the state of bits 0 turn from 0 to 1)-(in step 5)

7)
Coming to the compare function. It takes two numbers a and b. It encodes a in qra register and b in qrb register. Then it appends the problem solving circuit(compare) on all the qubits, starting with qra and qrb-... The bits.

8)
If b>a, we get 1010(refer the circuit given in the code) that is read from right to left. So essentially the 2nd and 4th qubit represents b. ie... b>a.

Accordingly, if a>b, we get 0101. Here the 1st and the 3rd qubit tells us that a>b.

Else, if a=b, then we see none of the auxiliary qubits is 1. Doesn't matter of both the numbers are 0 or 1.

from qiskit import QuantumCircuit, QuantumRegister, execute, Aer
from qiskit.visualization import plot_histogram

def bit_compare():
    qr = QuantumRegister(2, "bits")
    aux = QuantumRegister(2, "aux")
    
    qc = QuantumCircuit(qr, aux)
    qc.x(qr[1])
    qc.mcx(qr, aux[0])
    qc.x(qr[0])
    qc.x(qr[1])
    qc.mcx(qr, aux[1])
    qc.x(qr[0])
    
    return qc

bit_compare().draw()

<img width="182" alt="ckt1" src="https://user-images.githubusercontent.com/127426256/224118328-013dded6-3f5c-4bb0-ab3c-514f827ed09c.png">


qr = QuantumRegister(2, "bits")
aux = QuantumRegister(2, "aux")

qc = QuantumCircuit(qr, aux)
qc.x(qr[1])
qc.mcx(qr, aux[0])
qc.x(qr[1])

qc.draw()
<img width="113" alt="ckt2" src="https://user-images.githubusercontent.com/127426256/224119000-e38cf1e0-e71c-4422-8861-1a0c8d629730.png">

qr = QuantumRegister(2, "bits")
aux = QuantumRegister(2, "aux")

qc = QuantumCircuit(qr, aux)
qc.x(qr[0])
qc.mcx(qr, aux[1])
qc.x(qr[0])

qc.draw()
<img width="125" alt="ckt3" src="https://user-images.githubusercontent.com/127426256/224119373-56063ce8-a8a5-47fb-8011-a1e1133af4b1.png">
def compare(a, b):
    qra = QuantumRegister(1, "a")
    qrb = QuantumRegister(1, "b")
    qraux = QuantumRegister(2, "aux")

    qc = QuantumCircuit(qra, qrb, qraux)

    qc.append(encode(a), [*qra])
    qc.append(encode(b), [*qrb])

    qc.append(bit_compare(), [*qra, *qrb, *qraux])

    # Tell Qiskit how to simulate our circuit
    backend = Aer.get_backend('statevector_simulator') 

    # Do the simulation, returning the result
    result = execute(qc,backend, shots=1000).result()

    # get the probability distribution
    counts = result.get_counts()
    
    return counts
    
    counts = compare(0,1)
plot_histogram(counts)
<img width="443" alt="histo 1" src="https://user-images.githubusercontent.com/127426256/224119748-b2c35190-fdca-46d6-b7c0-094d7d9aaa19.png">

counts = compare(1,0)
plot_histogram(counts)

<img width="403" alt="histo 2" src="https://user-images.githubusercontent.com/127426256/224120130-e65ce8a0-ad63-4295-944d-3c6d005818c3.png">

counts = compare(1,1)
plot_histogram(counts)
<img width="396" alt="histo3" src="https://user-images.githubusercontent.com/127426256/224120465-7613d1de-9a98-4ca1-b5dc-e97f5cfe655f.png">












