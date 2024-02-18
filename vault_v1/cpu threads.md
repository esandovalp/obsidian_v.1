#CS
# What are CPU threads?

- **The Basics:** Think of a CPU thread as a virtual worker within your computer's processor (CPU). It's a sequence of instructions that the CPU can execute independently.
- **Multitasking Made Easy:** Threads are what allow your computer to do several things at once. For instance, you can write a document, listen to music, and have your email downloading updates in the background—all seemingly simultaneously.
- **CPU Cores vs. Threads:**    
    - **CPU Cores:** The physical hardware units within a CPU that actually execute instructions. Think of them as separate brains.
    - **Threads:** Virtual workers within a core, handling different tasks. One core can potentially run multiple threads.

# How do threads work?

1. **Dividing work:** When a program runs, it can be broken down into smaller tasks, or threads.
2. **Smart Scheduling:** The operating system cleverly manages these threads, assigning them to available CPU cores or switching between them rapidly within a single core. This gives the illusion of things happening at the same time.
3. **Hyper-threading:** A technology (mainly Intel CPUs) that makes a single physical core appear as two logical cores to the operating system. This allows each core to execute two threads simultaneously, further boosting performance.

# Why are threads important?

- **Responsiveness:** Apps feel snappier because your computer can quickly switch between different things you're doing.
- **Efficiency:** By dividing tasks into threads, CPUs can better utilize their resources, avoiding idling while some parts of a program wait for others to finish.
- **Demanding Software:** Threads are a lifesaver for power-hungry applications like video editing, 3D modeling, and complex simulations, which thrive on having more threads available.

# Analogy

Imagine your CPU as a restaurant kitchen:
- **CPU cores** are like individual chefs. More chefs equal more dishes cooked at once.
- **Threads** are like the different orders each chef can juggle. A skilled chef can manage multiple orders and prep items in parallel.
- **Hyper-threading** gives each chef a sous-chef, splitting complex tasks amongst two workers within the same 'kitchen space'.

# Datos de la clase sobre threads

- Si hay threads que actúan sobre un mismo programa ocasiona que puedan haber distintos resultados y se convierta en no determinístico.
- Sincronía: existen ciertos periodos para sincronizarse, hay procesos que están esperando, en específico los hilos.
- No determinismo: múltiples hilos lo puede ocasionar. No controlamos como se comportan los hilos.
- Por procesos hay al menos un hilo. Ahora trabajaremos con hilos (también se le conocen como procesos ligeros)
- Existe un número adecuado de hilos por programa, arquitectura, etc