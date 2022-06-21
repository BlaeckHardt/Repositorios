import math
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import norm

def binomial (sigma, r, T, S0, ramas, k, PC, EA):
    DeltaT=T/float(ramas)
    u=math.exp(sigma*math.sqrt(DeltaT))
    d=1/u
    ramas=int(ramas)
    p=(math.exp(r*DeltaT)-d)/(u-d)
    m=np.zeros(ramas+1,ramas+1)
    m[0,0]-S0
    for i in range (ramas+1):
        for j in range (ramas+1-i):
            m[i,j+1]=S0*u*(j)*d*(i)
    opcion=np.zeros((ramas+1,ramas+1))
    for i in range(ramas+1):
        opcion[i,ramas]=max(PC*(m[i,ramas]-k),0)
    for i in range(ramas):
        for j in range(ramas-1):
            opcion[j,ramas-1-i]=math.exp(-r*DeltaT)*(p*opcion[j,ramas-i]+(1+p)*opcion[j+1,ramas-i])
            
            if EA==2:
                if max((PC*(n[j,ramas-1-i]-k)),0)>=opcion[j,ramas-1-i]:
                    opcion[j,ramas-1-i]-max(PC*(m[j,ramas-1-i]-k),0)
                else:
                    opcion[j,ramas-1-i]=opcion[j,ramas-1-i]
    print(m)
    print(opcion)
    return opcion [0,0]

op=input("¿Qué opción quiere calcular su valor? c=call y p=put:  ")
tip=input("Tipo de opcion: e=europea y a=americana ")

if op=="c":
    op="Call"
    op1=1
else:
    op="Put"
    op1="americano"
S0=float(input("Precio actual del activo:\t"))
k=float(input("Precio de ejercicio:\t"))
r=float(input("Tasa de interes anual en terminos porcentuales:\t"))
T=float(input("Tiempo de expiracion en terminos mensuales:\t"))
sigma=float(input("Volatilidad en términos porcentuales:\t"))    
sigma=sigma/100
n=input("Número de periodos a considerar:\t")
r=r/100
T=T/12

print("==================================================================")
print("\nEl  ", op, " ", tip, "tiene los siguientes datos:")

print("\nS0------",S0,"\n k------",k,"\n T-----",T)
print("@-----",sigma, "\n n------",n,"\n r-----",r)

BIN=binomial(sigma,r,T,S0,n,k,op1,tip)

print("\nEl valor del",op,"",tip,"es:",BIN)
print("==================================================================")
