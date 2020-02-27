# Monitores en Python

La idea básica de un monitor es que funciona como un `Lock` pero que puede ser adquirido cuando se da una determinada condición:

```python
monitor = threading.Condition()

# Consumir un ítem
with monitor:
    while not hay_un_item():
        monitor.wait()
    consumir_un_item()

# Producir un ítem
with monitor:
    hacer_un_item()
    monitor.notify()


# el with es una forma fácil de hacer acquire y release.
# Hubiera sido lo mismo hacer así:
monitor.acquire():
try:
    hacer_un_item()
    monitor.notify()
finally:
    monitor.releas()
```

El manejo básico de monitores es el siguiente:
```python
monitor = threading.Condition() #crea el monitor. Se le puede pasar como parámetro un Lock en particular
monitor.acquire() # mismo que para semáforos
monitor.release() # mismo que para semáforos
monitor.wait() # esperar hasta recibir señal
monitor.notify() # dar señal a algún thread que está esperando
monitor.notifyAll() # dar señal a todos los threads que están esperando
```

## Productor y consumidores
En `prod-cons.py` hay un ejemplo a modo de guía de uso de un monitor. Miralo un rato y asegurate de entenderlo. ¿Por qué el thread que consume sigue consumiendo hasta que se acaban los ítems?

Ahora modificá `prod-cons.py` para que haya varios threads consumiendo.

Ahora agregá la posibilidad de que haya consumidores que consuman distintas cantidades (y no siempre un ítem).