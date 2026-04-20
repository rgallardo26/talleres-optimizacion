# talleres-optimizacion
import numpy as np
from scipy.optimize import minimize

#--------------------------------------
# DATOS DEL PROBLEMA


# Disponibilidad de recursos
A = 120
B = 80
C = 150

# GANACIA
PRECIO_F1 = 180
PRECIO_F2 = 220

#--------------------------------------
# FUNCIÓN OBJETIVO (MAXIMIZAR)

def funcion_objetiva(x):
    F1, F2 = x
    return -(PRECIO_F1 * F1 + PRECIO_F2 * F2)

#--------------------------------------
# RESTRICCIONES


def restriccion_A(x):
    F1, F2 = x
    return A - (3*F1 + 4*F2)

def restriccion_B(x):
    F1, F2 = x
    return B - (2*F1 + 1*F2)

def restriccion_C(x):
    F1, F2 = x
    return C - (5*F1 + 3*F2)

LIMITES = [
    {'type': 'ineq', 'fun': restriccion_A},
    {'type': 'ineq', 'fun': restriccion_B},
    {'type': 'ineq', 'fun': restriccion_C}
]

#--------------------------------------
# LÍMITES 
NEGATIVOS = [(0, None), (0, None)]

# Punto inicial
x0 = [1, 1]

resultado = minimize(
    funcion_objetiva,
    x0,
    method='SLSQP',
    bounds=NEGATIVOS,
    constraints=LIMITES
)

F1_opt, F2_opt = resultado.x
ganancia_max = -resultado.fun



RESPUESTA_A="A) las variables de decision corresponde a la varible x que quivale a la cantidad de fertilizante F1 y F2 produciodos con el lote disponible de quimicos"
RESPUESTA_B="B) La funcion objetiva es la correspondiente a la funcion de funcion_objetiva() donde es la funcion a maximizar es Z =  180f1 + 220f2"
RESPUESTA_C="C) Las restricciones correpondiente al inventario esta en la funcion llamda LIMITES donde se plasman los siguientes limites A=<120, B=<80, C=<120 Y en la funcion de NEGATIVOS se plasman los siguientes limites A>=0,B>=0,C>=0"
RESPUESTA_D="D) ES un ploblema de tipo lineal porque su funcion objetiva es de grado 1 y las variables no se multiplican entre si y es de tipo continuo pq las unidades son toneladas por ende puede trabajar numeros decimales no esta limitado a solo numeros enteros"

print("============================================================================================================")
print("PRIMER PUNTO")
print("============================================================================================================")
print(RESPUESTA_A)
print(RESPUESTA_B)
print(RESPUESTA_C)
print(RESPUESTA_D)

print("E) la distribucuion mas optima del problema es F1=",F1_opt, "y F2=",F2_opt)


# segundo punto
# Datos
S=200
M=80

#FUNCION OBJETIVO

def produccion_semanal(X,S_0,M_0):
    while X>0:
        S_N=0.6*S_0+0.2*M_0+40
        M_N=0.1*S_0+0.5*M_0+20
        S_0=S_N
        M_0=M_N
        X-=1
    return S_N,M_N


def Equilibrio_2():
    X=1
    while X>0:
        if produccion_semanal(X,S,M) == produccion_semanal(X+1,S,M):
            respuestas=X
        
            X=0
        else:
            respuestas=0
            X+=1
    return respuestas
            

RESPUESTA_A="A) Las variantes dependientes son el numero de sillas y mesas que dependen del tiempo y el tiempo es nuestra variable independiente"
RESPUESTA_B="B) El problema es de tipo lineal porque su funcion objetiva es primer grado y sus variables no se multiplican y es discreto porque depende de intervalos de tiempo y no avanza de manera continua"

print("============================================================================================================")
print("SEGUNDO PUNTO")
print("============================================================================================================")
print(RESPUESTA_A)
print(RESPUESTA_B)
print("C) La produccion en la semana 1 es de ",produccion_semanal(1,S,M)," y en la semana 2 de ",produccion_semanal(2,S,M))
print("D) el punto de equilibrio esta en sillas y mesas respectivamente=",produccion_semanal(Equilibrio_2(),S,M))


#tercer punto 
#DATOS
B=0.00003
I=50
Y=0.1
def infectados(x,B,I,Y):
    while x>0:
        DI=B*(10000-I)*I-(I*Y)
        I=DI
        x-=1
    return I

def Equilibrio_3():
    X=1
    while X>0:
        if infectados(X,B,I,Y) == infectados(X+1,B,I,Y):
            respuestas=infectados(X,B,I,Y)
            X=0
        else:
            respuestas=0
            X+=1
    return respuestas

#equilibrio 2 
#DESPEJAMOS Y NOS DA 10000-Y/B
 

print("============================================================================================================")
print("TERCER PUNTO")
print("============================================================================================================")

RESPUESTA_A="A) Las variables dependientes son el numero de infectados y la variable independiente es el tiempo y sus parametros son la poblacion inicial(I), indice de recuperacion(Y), indice de infenccion(B) y su poblacion MAXIMA"
Respuesta_B="B) el problema no es lineal porque es de segundo grado debido a que en la parte de (10000-I)I resultan en I^2"
Respuesta_C="C) la condicion inicial es la poblacion inicial de infectados de 50"
Respuesta_E="E) el pimer equilibrio se presenta cuando no existen infectados el segundo nos indica un valor de infectados proximo a 6666 donde el nivel de recuperados por dia nivela y mantiene esta valor constante"

print(RESPUESTA_A)
print(Respuesta_B)
print(Respuesta_C)
print("D) los puntos de equilibrio son I=",Equilibrio_3(),"Y I=10000-Y/B")
print(Respuesta_E)


#cuarto PUNTO
X=2
K=1000
R=0.4
H=80
P_0=400


def poblacion(X,K,R,H,P_0):
    while X>0:
        P_N=P_0+R*P_0*(1-P_0/K)-H
        P_0=P_N
        X-=1
    return P_N


def Equilibrio_4():
    X=1
    while X>0:
        if poblacion(X,K,R,H,P_0) == poblacion(X+1,K,R,H,P_0):
            respuestas=X
        
            X=0
        else:
            respuestas=0
            X+=1
    return respuestas
            
print("============================================================================================================")
print("CUARTO PUNTO")
print("============================================================================================================")

RESPUESTA_A="A) Las variables dependientes son el numero de peces en el estanque y la variable independiente es el tiempo y sus parametros son la poblacion inicial(P_O), indice de caza(H), indice de reproduccion(R) y su poblacion maxima"
Respuesta_B="B) el problema no es lineal porque es de segundo grado y es discreta debido a que el proximo aaño depende directamente del año anterior porlo que no pueda realizar altos entre los años"


print(RESPUESTA_A)
print(Respuesta_B)
print("C) la poblacion en el año 1 es de",poblacion(1,K,R,H,P_0),"y en al año 2 es de", poblacion(2,K,R,H,P_0))
print("D) el punto de equilibrio se alcanza con una poblacion de ",poblacion(Equilibrio_4(),K,R,H,P_0))
print("E) si la poblacion es menor al equilibrio inestable esto significa que el indice de caza es demasiado supeior a su reproduccion por ende la demanda no se podra satisfacer y se acabara con todos los peces en el estanque")
