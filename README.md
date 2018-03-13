# 32-bit-hash-function
Creating a 32 bit hash function, generates hash value while scanning the directory. Also, checks for collisions.
import os
import os.path 

try:
    os.remove("output_hash.txt")
    
except:
    pass

def char2byte(char):   # Converting the each character to bytes - 8 bits
    return bin(ord(char))[2:].zfill(8)

def circular_shift(bits): # Circular shifts by 4 places
    bit_l = bits[:4]
    bit_r = bits[4:]
    bits = bit_r + bit_l
    return bits

def xor_op(h,b2):  # snippet to perform xor on binary strings
    h_8bits = h[24:]
    #print(h_8bits)
    h_8xor =''
    for i in range(len(b2)):
        if b2[i] == '0'and h_8bits[i] == '0':
            h_8xor = h_8xor + '0'
        if b2[i] == '1'and h_8bits[i] == '1':
            h_8xor = h_8xor + '0'
        if b2[i] == '0'and h_8bits[i] == '1':
            h_8xor = h_8xor + '1'
        if b2[i] == '1'and h_8bits[i] == '0':
            h_8xor = h_8xor + '1'

    h_l = h[:24]
    h = h_l + h_8xor
    return h

def bin2str(binary):
    return int(binary,2)
       
def xor_file_con(con,h):   # performing circular shifts on hash value and xor on every byte through the contents of file
    for i in con:
        c = char2byte(i)
        c_h = circular_shift(h)
        x_h = xor_op(c_h, c)
        h = x_h

    return h
        
def file_write(hash_val, f):
    f.write(hash_val)
    f.write('\n')
    

def hashing():       # performing xor and circular shifts and stroing the hash values in a output file
    h = '0'.zfill(32)
    path =r'C:\Users\user\AppData\Local\Programs\Python\Python36-32\directory'
    fo = open("output_hash.txt","w")
    for filename in os.listdir(path):
        file = os.path.join(path,filename)
        f = open(file, 'r')
        if f.mode == 'r':
            con = f.read()
            f.close()
        n_h = str(bin2str(xor_file_con(con,h)))  # converting the obtained integer to a string
        file_write(n_h,fo)
    fo.close()       
    


hashing()


fo_r = open('output_hash.txt','r')
l = []
s =''
if fo_r.mode == 'r':
    hash_chrlist = fo_r.read()
    hash_chrlist.strip()
    fo_r.close()

for i in hash_chrlist: # reading the file contents an storing the values of hash in a list to check for collisions
    #print(i)
    if i == '\n':
        l.append(s.strip('\n'))
        s = ''    
    s = s + i
      
print(l)
def FindCollisions(l):  # To check if there is any collsions by checking for duplicate hash values in the list stored 
    unique = set(l)  
    if len(unique) == len(l):
        return False
    else:
        return True

if FindCollisions(l):
    print('There are collisions!')
else:
    print('There are no collisions!')

