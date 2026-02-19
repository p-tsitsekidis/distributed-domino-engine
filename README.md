# Distributed Domino Engine

A concurrent, client-server implementation of the classic Domino game built in pure Java. 

This project was built to demonstrate core distributed system concepts, avoiding high-level networking libraries in favor of raw Java Sockets. It features a custom event-driven network protocol, a centralized mathematical state engine, and thread-safe graphical/CLI clients.

## Technical Architecture & Highlights

### 1. Event-Driven Network Protocol (Command Pattern)
Instead of heavily nested `switch` statements, the system employs the **Command Design Pattern** to parse incoming network streams. 
* String headers are mapped directly to method references via a `HashMap<String, Runnable>`.
* This allows the client to asynchronously and cleanly process incoming state updates from the server without blocking.

### 2. State Machine & Algorithmic Rigor
The `GameEngine` acts as the single source of truth, eliminating client-side trust issues. 
* **Data Structures:** Utilizes a double-ended queue (`Deque<Tile>`) to represent the line of play, allowing for highly efficient **O(1)** insertions at both edges of the board.
* **Validation:** Enforces strict mathematical rules for tile placement, automated stock drawing, and win-condition calculations.

### 3. Concurrency & UI Separation
* Implements strict thread separation. Background threads handle blocking network I/O operations, ensuring the user interface remains responsive.
* Utilizes `Platform.runLater()` to safely push state mutations from the network listening thread to the JavaFX application thread.

## System Layers

The architecture is strictly decoupled into three primary layers:
1. **Game Engine (`gr.uop.GameEngine`):** Pure Java domain logic handling board state, permutations, and scoring. Zero network awareness.
2. **Server (`gr.uop.DominoServer`):** Manages the TCP socket lifecycle, dual-client I/O streaming, and broadcasts game state mutations.
3. **Clients (`gr.uop.JavaFXClient` / `CommandLineClient`):** Thin clients that maintain no local game logic; they simply parse server commands and render the state.

## Getting Started

### Prerequisites
* Java Development Kit (JDK) 8 or higher
* JavaFX SDK (if running the GUI client)

### Running the System

1. **Start the Server:**
   Compile and run `Server.java`. Choose either `Local` (CLI only) or `Networked Server` mode. The server binds to port `7777` by default.
2. **Start the Clients:**
   Run `JavaFXClient.java` for the graphical interface, or `CommandLineClient.java` for the terminal interface. 
3. **Connect:** Enter the server's IP address (use `localhost` for local testing) when prompted by the client.

## Future Roadmap

* **Serialization Migration:** Transitioning the custom space-separated string protocol to structured serialization (e.g., JSON via Jackson, or Google Protocol Buffers) for stricter type safety.
* **Fault Tolerance:** Implementing heartbeat mechanisms to gracefully recover from sudden client disconnects without crashing the server loop.

## Credits

Special thanks to [Manos Schoinoplokakis](https://github.com/Manos1Dev) for providing the JavaFX implementation of the game. Their contribution was essential in building the graphical interface for this project.


## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE.md) file for details.
