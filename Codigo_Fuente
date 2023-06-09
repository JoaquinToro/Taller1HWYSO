#Integrantes: Agustín Cardenas Y Joaquín Toro
import pygame
import time
import threading

matriz=[]
for i in range(5):
   piso=[]
   for j in range(50):
      vector=[]
      for j in range(30):
          vector.append('.')
      piso.append(vector)
   matriz.append(piso)

pygame.init()
fd = open("C:\\Users\Miles\Desktop\hardware_y_sistemas_operativos_Taller_1\Datos_Laberinto.txt","r")
datos = fd.readlines()
for dato in datos:
   pos = dato.split(',')
   
   matriz[int(pos[2])][int(pos[1])][int(pos[0])] = pos[3][0:1]
   print(pos[2] + ',' + pos[1] + ',' + pos[0] + ' = ' + pos[3][0:1])
   

semDupli = threading.Semaphore(10)
semMutex = threading.Semaphore(1)

ventana = pygame.display.set_mode((0, 0), pygame.FULLSCREEN)

colorfondo = (0,0,255)
colormuralla = (255,0,0)
colorpasillo = (255,255,255)
colorVentana = (100,20,60)
colorEscalera = (0,255,0)
colorOrificio = (0,0,0)
colorSuper = (60,70,80)

cantPisos=5
cantColumnas = 30   
cantFilas = 50       
tamanocuadros = 8   
                     

posarriba = 100
posizquierda = 0
ancho = cantColumnas * tamanocuadros  
alto = cantFilas * tamanocuadros                                          

pygame.draw.rect(ventana,colorfondo,( (posizquierda,posarriba,ancho,alto)))


def abajo(z,y,x,r,g,b):
  global matriz
  i = y
  semDupli.acquire()
  while(i<=49) and matriz[z][i][x]== '.':
    pygame.event.get()
    matriz[z][y][x] = '-'
    
    pygame.draw.rect(ventana,colorSuper,((x*tamanocuadros+(posizquierda + z*250),i*tamanocuadros+posarriba,tamanocuadros,tamanocuadros)))
    pygame.display.update()
    
    if(x>0) and (matriz[z][i][x-1]=='.'):
      semMutex.acquire()
      hebraIzquierda = threading.Thread(target=izquierda,args=(z,i,x-1,60,70,80))
      hebraIzquierda.start()
      semMutex.release() 
      
    if(x<29) and (matriz[z][i][x+1]=='.'):
      semMutex.acquire()
      hebraDerecha = threading.Thread(target=derecha,args=(z,i,x+1,60,70,80))
      hebraDerecha.start()
      semMutex.release()  

    if(z<=4) and (i<49) and(matriz[z][i+1][x] == 'E'):
      semMutex.acquire()  
      hebraAbajo = threading.Thread(target=abajo,args=(z+1,i+1,x,60,70,80))
      hebraAbajo.start()
      semMutex.release()  
    
    if(z>0) and (i<49) and (matriz[z][i+1][x] == 'O'):
      semMutex.acquire()  
      hebraAbajo = threading.Thread(target=abajo,args=(z-1,i+1,x,60,70,80))
      hebraAbajo.start()
      semMutex.release()  
      
    time.sleep(0.05)
    i+=1
    
  semDupli.release()
  
  if i < 50 and matriz[z][i][x] == 'V':
    print('Salida encontrada en posicion (',z,',',x,',',y,')')

def arriba(z,y,x,r,g,b):
  global matriz
  i = y
  semDupli.acquire()
  while(i>=0) and (matriz[z][i][x]== '.'):
    pygame.event.get()
    matriz[z][i][x] = '-'
    
    pygame.draw.rect(ventana,colorSuper,((x*tamanocuadros+(posizquierda + z*250),i*tamanocuadros+posarriba,tamanocuadros,tamanocuadros)))
    pygame.display.update()
    
    if(x>0) and (matriz[z][i][x-1]=='.'):
      semMutex.acquire()
      hebraIzquierda = threading.Thread(target=izquierda,args=(z,i,x-1,60,70,80))
      hebraIzquierda.start()
      semMutex.release()  
      
    if(x<29) and (matriz[z][i][x+1]=='.'):
      semMutex.acquire()  
      hebraDerecha = threading.Thread(target=derecha,args=(z,i,x+1,60,70,80))
      hebraDerecha.start()
      semMutex.release()  

    if(z<=4) and (i>0) and(matriz[z][i-1][x] == 'E'):
      semMutex.acquire()  
      hebraAbajo = threading.Thread(target=abajo,args=(z+1,i-1,x,60,70,80))
      hebraAbajo.start()
      semMutex.release()  
    
    if(z>0) and (i>0) and (matriz[z][i-1][x] == 'O'):
      semMutex.acquire()  
      hebraAbajo = threading.Thread(target=abajo,args=(z-1,i-1,x,60,70,80))
      hebraAbajo.start()
      semMutex.release()  
      
    time.sleep(0.05)
    i-=1
    
  semDupli.release()
  
  if i >=0 and matriz[z][i][x] == 'V':
    print('Salida encontrada en posicion (',z,',',x,',',y,')')

def derecha(z,y,x,r,g,b):
  global matriz
  i=x
  semDupli.acquire()
  while (i<=29) and (matriz[z][y][i] == '.'):
    pygame.event.get()
    matriz [z][y][i] = '-'
    
    pygame.draw.rect(ventana,colorSuper,((i*tamanocuadros+(posizquierda + z*250),y*tamanocuadros+posarriba,tamanocuadros,tamanocuadros)))
    pygame.display.update()
    
    if(y>0) and (matriz[z][y-1][i] == '.'):
      semMutex.acquire()  
      hebraArriba=threading.Thread(target=arriba,args=(z,y-1,i,60,70,80))
      hebraArriba.start()
      semMutex.release()  
      
    if(y<49) and (matriz[z][y+1][i] == '.'):
      semMutex.acquire()  
      hebraAbajo=threading.Thread(target=abajo,args=(z,y+1,i,60,70,80))
      hebraAbajo.start()
      semMutex.release()  

    if(z<4) and(i<29) and(matriz[z][y][i+1] == 'E'):
      semMutex.acquire()  
      hebraAbajo=threading.Thread(target=abajo,args=(z+1,y,i+1,60,70,80))
      hebraAbajo.start()
      semMutex.release()  
      
    if(z>0) and (i<29)and (matriz[z][y][i+1] == 'O'):
      semMutex.acquire()  
      hebraAbajo=threading.Thread(target=abajo,args=(z-1,y,i+1,60,70,80))
      hebraAbajo.start()
      semMutex.release()  
      
    time.sleep(0.05)  
    i+=1
    
  semDupli.release()

   
  if(i<30) and matriz[z][y][i] == 'V':
    print('Salida encontrada en posicion (',z,',',x,',',y,')')

def izquierda(z,y,x,r,g,b):
  global matriz
  i=x
  semDupli.acquire()
  while (i>=0) and (matriz[z][y][i]== '.'):
    pygame.event.get()
    matriz [z][y][i] = '-'
    
    pygame.draw.rect(ventana,colorSuper,((i*tamanocuadros+(posizquierda + z*250),y*tamanocuadros+posarriba,tamanocuadros,tamanocuadros)))
    pygame.display.update()
    if(y>0) and  (matriz[z][y-1][i] == '.'):
      semMutex.acquire()  
      hebraArriba=threading.Thread(target=arriba,args=(z,y-1,i,60,70,80))
      hebraArriba.start()
      semMutex.release()  
      
    if(y<49) and (matriz[z][y+1][i] == '.'):
      semMutex.acquire()  
      hebraAbajo=threading.Thread(target=abajo,args=(z,y+1,i,60,70,80))
      hebraAbajo.start()
      semMutex.release()  
      
    if(z<4) and (i>0) and(matriz[z][y][i-1] == 'E'):
      semMutex.acquire()  
      hebraAbajo=threading.Thread(target=abajo,args=(z+1,y,i-1,60,70,80))
      hebraAbajo.start()
      semMutex.release()  
      
    if(z>0) and (i>0) and (matriz[z][y][i-1] == 'O'):
      semMutex.acquire()  
      hebraAbajo=threading.Thread(target=abajo,args=(z-1,y,i-1,60,70,80))
      hebraAbajo.start()
      semMutex.release()  
      
    time.sleep(0.05)
    i-=1
  
  semDupli.release()
  if(i>=0) and matriz[z][y][i] == 'V':
    print('Salida encontrada en posicion (',z,',',x,',',y,')')

for z in range(cantPisos):
    pygame.draw.rect(ventana,colorfondo,(((posizquierda + z*250),posarriba,ancho,alto)))
    for x in range(cantColumnas):
         for y in range(cantFilas):
            pygame.event.get()
            if (matriz[z][y][x] == '.'):
               color = colorpasillo
            elif(matriz[z][y][x] == '-'):
                color = colorSuper
            elif (matriz[z][y][x] == 'X'):
               color = colormuralla
            elif (matriz[z][y][x] == 'V'):
               color = colorVentana
            elif (matriz[z][y][x] == 'E'):
               color = colorEscalera
            elif (matriz[z][y][x] == 'O'):
               color = colorOrificio
            pygame.draw.rect(ventana,color,( x*tamanocuadros+(posizquierda + z*250),y*tamanocuadros+posarriba,tamanocuadros,tamanocuadros))
            pygame.display.update()
            
hebraInicial = threading.Thread(target=abajo,args=(0,0,0,60,70,80))
hebraInicial.start()
