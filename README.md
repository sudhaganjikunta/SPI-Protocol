
# SPI-Protocol

This project implements a **Serial Peripheral Interface (SPI)** communication system in Verilog HDL, showcasing how a master device and a slave device exchange data in a synchronous, full-duplex manner.

---

## 🔑 Modules

### SPI Master (`spi_master.v`)
**Inputs:**
- `clk` – System clock
- `rst` – Reset signal
- `start` – Initiates communication
- `data` – 8-bit data to send
- `miso` – Data received from slave

**Outputs:**
- `mosi` – Data sent to slave
- `sclk` – Generated serial clock
- `ss` – Slave select
- `data_out` – 8-bit data received from slave
- `done` – Indicates completion of transfer

**Functionality:**
- Implements a finite state machine (FSM) with states: `IDLE`, `LOAD`, `TRANSFER`, `DONE`.
- Loads data into a shift register and transmits bit-by-bit to the slave.
- Simultaneously receives data from the slave into `recv_reg`.
- Provides a `done` signal when the transaction completes.

---

### SPI Slave (`spi_slave.v`)
**Inputs:**
- `sclk` – Serial clock from master
- `rst` – Reset signal
- `ss` – Slave select (active low)
- `mosi` – Master Out Slave In (data from master)

**Outputs:**
- `miso` – Slave Out Master In (data to master)
- `slave_received` – 8-bit register holding received data

**Functionality:**
- Captures incoming data from the master on the rising edge of `sclk`.
- Sends data back to the master on the falling edge of `sclk`.
- Uses a shift register (`shift_in`) for incoming data and (`shift_out`) for outgoing data.
- Transaction begins when `ss` goes low.

---

## ⚙️ Features
- Full-duplex communication (master sends while slave responds).
- MSB-first data transfer.
- Reset handling for clean initialization.
- Simple FSM-based master control logic.
- Demonstrates synchronization between master and slave using `sclk`.

---

## 🚀 Simulation Results

When simulated with a testbench:

- The **master** successfully transmits an 8-bit word to the slave via `mosi`.  
- The **slave** captures the incoming bits into `shift_in` and updates `slave_received` once the full byte is received.  
- Simultaneously, the **slave** responds with its predefined `shift_out` data via `miso`.  
- The **master** collects this response into `recv_reg` and outputs it as `data_out` once the transaction completes.  
- The `done` signal goes high, indicating the end of communication.  

### Example Waveform Behavior:
- `ss` goes low → transaction starts.  
- `sclk` toggles → data is shifted bit-by-bit.  
- `mosi` carries master’s data → slave receives it.  
- `miso` carries slave’s data → master receives it.  
- After 8 clock cycles → both devices have exchanged one byte.  
- `done` goes high → master signals completion.  

---

## 📘 Applications
- Educational use for learning **SPI protocol** and Verilog HDL.
- Basis for extending SPI to multiple slaves.
- Can be integrated into larger FPGA projects requiring serial communication.

---




