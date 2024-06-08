# Turing Machine Simulation

## Simulación de una Máquina de Turing en Python

"""
En este script, vamos a crear una simulación de una Máquina de Turing. Una Máquina de Turing es un modelo matemático que describe una máquina hipotética que manipula símbolos en una cinta de acuerdo con un conjunto de reglas. Se utiliza para entender los límites de lo que puede ser computado.
"""

### Partes de una Máquina de Turing
"""
1. **Cinta**: Un arreglo infinito de celdas que puede contener símbolos.
2. **Cabezal**: Se mueve a la izquierda o a la derecha a lo largo de la cinta y lee/escribe símbolos.
3. **Estados**: Un conjunto finito de estados en los que la máquina puede estar.
4. **Función de Transición**: Define las reglas para mover el cabezal, cambiar estados y escribir en la cinta.
"""

### Paso 1: Definir la Cinta

"""
Primero, definimos la clase que representa la cinta de la Máquina de Turing.
"""

class Tape:
    def __init__(self, tape_string=''):
        self.tape = list(tape_string)
        self.position = 0

    def __str__(self):
        return ''.join(self.tape)

    def read(self):
        if self.position < 0 or self.position >= len(self.tape):
            return '_'
        return self.tape[self.position]

    def write(self, char):
        if self.position < 0:
            self.tape = [char] + ['_'] * (-self.position - 1) + self.tape
            self.position = 0
        elif self.position >= len(self.tape):
            self.tape.extend(['_'] * (self.position - len(self.tape) + 1))
            self.tape[self.position] = char
        else:
            self.tape[self.position] = char

    def move_left(self):
        self.position -= 1
        if self.position < 0:
            self.tape.insert(0, '_')
            self.position = 0

    def move_right(self):
        self.position += 1
        if self.position >= len(self.tape):
            self.tape.append('_')

### Explicación del Código
"""
- **Constructor (`__init__`)**: Inicializa la cinta con un string y establece la posición inicial del cabezal.
- **Leer (`read`)**: Lee el símbolo en la posición actual del cabezal.
- **Escribir (`write`)**: Escribe un símbolo en la posición actual del cabezal.
- **Mover a la izquierda (`move_left`)**: Mueve el cabezal una posición a la izquierda.
- **Mover a la derecha (`move_right`)**: Mueve el cabezal una posición a la derecha.
"""

### Paso 2: Definir la Máquina de Turing

"""
Definimos la clase para la Máquina de Turing, que incluye la cinta, el conjunto de estados y la función de transición.
"""

class TuringMachine:
    def __init__(self, tape_string, initial_state, final_states, transition_function):
        self.tape = Tape(tape_string)
        self.state = initial_state
        self.final_states = final_states
        self.transition_function = transition_function

    def step(self):
        char = self.tape.read()
        if (self.state, char) in self.transition_function:
            new_state, new_char, direction = self.transition_function[(self.state, char)]
            self.tape.write(new_char)
            self.state = new_state
            if direction == 'L':
                self.tape.move_left()
            elif direction == 'R':
                self.tape.move_right()
        else:
            raise ValueError("Transition not defined for ({}, {})".format(self.state, char))

    def run(self):
        while self.state not in self.final_states:
            self.step()
        return str(self.tape)

### Explicación del Código
"""
- **Constructor (`__init__`)**: Inicializa la máquina con la cinta, el estado inicial, los estados finales y la función de transición.
- **Paso (`step`)**: Ejecuta un paso de la máquina de Turing de acuerdo con la función de transición.
- **Ejecutar (`run`)**: Ejecuta la máquina hasta alcanzar un estado final.
"""

### Ejemplo de Uso

"""
Definimos una función de transición simple para demostrar cómo la máquina de Turing cambia de estado y modifica la cinta.
"""

# Función de transición: (estado_actual, simbolo_lectura) -> (nuevo_estado, nuevo_simbolo, direccion)
transition_function = {
    ('q0', '0'): ('q0', '0', 'R'),
    ('q0', '1'): ('q1', '0', 'R'),
    ('q1', '0'): ('q2', '1', 'R'),
    ('q1', '1'): ('q1', '1', 'R'),
    ('q2', '0'): ('q2', '0', 'R'),
    ('q2', '1'): ('q2', '1', 'R'),
}

# Estado inicial y estados finales
initial_state = 'q0'
final_states = {'q2'}

# Crear una Máquina de Turing con una cinta inicial
turing_machine = TuringMachine('0110', initial_state, final_states, transition_function)

# Ejecutar la máquina
final_tape = turing_machine.run()

print("Cinta final:", final_tape)

### Explicación del Ejemplo
"""
- **Función de Transición**: Define cómo la máquina cambia de estado y modifica la cinta.
- **Estado Inicial y Estados Finales**: Define el estado inicial y los estados que detendrán la máquina.
- **Ejecutar la Máquina**: Crea una instancia de la máquina con una cinta inicial y la ejecuta hasta alcanzar un estado final.
"""

"""
Este es un ejemplo básico de cómo simular una máquina de Turing en Python. Puedes expandir este código para incluir más estados, diferentes símbolos, y reglas de transición más complejas según sea necesario.
"""
