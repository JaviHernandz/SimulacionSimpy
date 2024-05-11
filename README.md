# SimulacionSimpy
#Proyecto de simulacion de eventos discretos
import simpy
import math
from random import randint

class server():

   def __init__(self, env, machine, my_id): #constructor que crea el objeto tipo car, necesita env, machine(servidor), id que identifica coche
        self.env = env
        self.id=my_id
        self.server=machine
        self.waiting=0 #se inicializa el tiempo de espera 
        self.env.process(self.running())# nicia el proceso, se le pone el hilo al titiritero, a un proceso se le liga siempre una funcion 
      

  def arrived_and_washed(self):
        llegada=randint(1,7)#distribucion de poisson se tiene que poner ahi, para que nos de lambdas de llegada
        yield self.env.timeout(llegada)
        start_waiting=self.env.now#tiempo de espera
        print('%s arriving at %d' % (self.id, self.env.now))
        rq=self.server.request()
        yield rq
        self.waiting=self.env.now - start_waiting
        print('%s total waiting time %d' % (self.id, self.waiting))
        yield self.env.timeout(2) #washing car
        self.server.release(rq)
        print('%s leaving at %d' % (self.id, self.env.now))

  def running(self):
        #while true
        yield self.env.process(self.arrived_and_washed())
        #capacity numero de servidores que se quieren simular 
        


