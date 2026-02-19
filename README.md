# Event-Driven-Domino-Server

A concurrent, client-server implementation of the classic Domino game built in Java. This project demonstrates core distributed system concepts, featuring a custom event-driven network protocol, a centralized game state engine, and supports both command-line and JavaFX graphical clients.

## Technical Highlights
* **Client-Server Architecture:** Utilizes Java Sockets for low-latency, TCP-based communication between the centralized server and remote clients.
* **Event-Driven Command Parsing:** Employs the Command Design Pattern (mapping string headers to method references via `HashMap<String, Runnable>`) to cleanly and asynchronously process incoming network events without heavily nested logic.
* **Thread Safety & UI Separation:** Ensures responsive user interfaces by strictly separating background network listening threads from the JavaFX application thread using `Platform.runLater()`.
* **Robust State Machine:** The `GameEngine` acts as the single source of truth. It enforces strict mathematical game rules, validates moves against a double-ended queue (`Deque<Tile>`) for $O(1)$ edge insertions, and automates stock drawing without relying on client-side trust.

## Architecture Overview

The system is decoupled into three primary layers:
1. **Game Engine (`gr.uop.GameEngine`)**: Pure Java logic handling the board state, tile validation, and scoring algorithms.
2. **Server (`gr.uop.DominoServer`)**: Manages the socket lifecycle, handles dual-client I/O streams, and broadcasts game state updates.
3. **Clients (`gr.uop.JavaFXClient` / `CommandLineClient`)**: Thin clients that parse server commands and render the state either via CLI or a JavaFX graphical interface.

## Getting Started

### Prerequisites
* Java Development Kit (JDK) 8+
* JavaFX SDK (for the GUI client)

### Running the Game
1. **Start the Server:**
   Compile and run `Server.java`. Choose either Local (CLI only) or Networked Server mode. By default, the server binds to port `7777`.
2. **Start the Clients:**
   Run `JavaFXClient.java` (GUI) or `CommandLineClient.java` (CLI). Enter the server's IP address (`localhost` for local testing) when prompted.

## Future Improvements
While the current architecture is fully functional for local and low-latency network play, future iterations would focus on production-grade robustness:
* **Serialization Migration:** Transitioning the custom space-separated string protocol to structured serialization (e.g., JSON via Jackson/Gson, or Google Protocol Buffers) for stricter type safety and easier extensibility.
* **Robust Fault Tolerance:** Implementing heartbeat mechanisms and robust `try-catch` socket closure handling to gracefully recover from sudden client disconnections or network drops without crashing the server loop.

## Credits

Special thanks to [Manos Schoinoplokakis](https://github.com/Manos1Dev) for providing the JavaFX implementation of the game. Their contribution was essential in building the graphical interface for this project.


## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE.md) file for details.
