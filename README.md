# Linear-Block-Code
# Aim
Write a simple python program to Generate Matrix, Codeword, Hamming weight, Syndrome matrix and find the error on received codeword using Linear block code. 
# Tools required
Google colab
# Program
```
import numpy as np

pb = []          # Parity matrix
h_dis = []
r_code = []
err = []

col = int(input("Enter the Parity bits : "))
row = int(input("Enter the Message bits : "))

# Generator matrix input
for i in range(row):
    p = list(map(int, input(f"Enter the row values {i+1} (Separated by space): ").split()))
    pb.append(p)

p_mat = np.array(pb, dtype=int)

# Identity matrix
Ik = np.eye(row, dtype=int)

# Generator matrix G = [P | I]
g_mat = np.hstack((p_mat, Ik))

# Codeword length and message length
k = row
n = g_mat.shape[1]

# Possible message bits
m = np.array([
    [1 if (i >> (k - j - 1)) & 1 else 0 for j in range(k)]
    for i in range(2 ** k)
])

# Codewords
c = np.mod(np.dot(m, g_mat), 2)

# Hamming weights
for row1 in c:
    h_dis1 = np.sum(row1)
    h_dis.append(h_dis1)

h_mat = np.array(h_dis).reshape(-1, 1)

# Minimum Hamming distance
d_min = np.min(np.sum(c[1:], axis=1))

# Parity Check Matrix H = [Pᵀ | I]
hp = np.hstack((p_mat.T,np.eye(n-k, dtype=int)))
ht = hp.T

print('**********')
print('The Generator Matrix is:')

for r in g_mat:
    print(" ".join(map(str, r)))

print('**********')
print('Message Bits\tCodeword\tHamming Weight')

code_word = np.hstack((m, c, h_mat))

for r in range(code_word.shape[0]):
    format_row = (
        " ".join(map(str, code_word[r, :k])) +
        '\t\t' +
        " ".join(map(str, code_word[r, k:n+k])) +
        '\t\t' +
        str(code_word[r, -1])
    )
    print(format_row)

print('**********')
print(f'Minimum Hamming distance : {d_min}')

# Parity Check Matrix
print('**********')
print('Parity Check Matrix')

for r in hp:
    print(" ".join(map(str, r)))

print('**********')
print('Parity Check Matrix Transpose')

for r in ht:
    print(" ".join(map(str, r)))

# Receive codeword
rc = list(map(int, input("Enter the error codeword : ").split()))
r_code.append(rc)

r_c = np.array(r_code)

# Syndrome Calculation
e = np.mod(np.dot(r_c, ht), 2)

print('**********')
print("Syndrome of given received codeword is : " +
      " ".join(map(str, e[0])))

print('**********')
print('Syndrome Matrix')

for i in range(n):
    combined_row = np.concatenate((ht[i, :], np.eye(n, dtype=int)[i, :]))
    print(" ".join(map(str, combined_row)))

# Find error position
for i in range(n):
    if np.array_equal(e[0], ht[i, :]):
        err = np.eye(n, dtype=int)[i, :]

print("The error position is : " + " ".join(map(str, err.astype(int))))

# Correct the error
add = np.mod(err + rc, 2)

print("The correct codeword is : " +
      " ".join(map(str, add.astype(int))))
```
# Output Waveform

<img width="520" height="671" alt="Screenshot 2026-05-20 090952" src="https://github.com/user-attachments/assets/94ebcf9a-9972-49ce-9031-d088dd897edf" />

## Calculation:
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/5633235f-4f8b-4850-8e90-e06427c68f92" />
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/f48dc18a-f0fd-4155-9e67-126a4b0109d4" />





# Results

Hence,the Linear Block Code was executed and tesed successfully.


