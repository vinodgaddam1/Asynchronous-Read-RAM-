# Asynchronous-Read-RAM

## 📌 1. Project Title
✅ **Parameterized Asynchronous-Read RAM (Sync Write / Async Read)**

---

## 📝 2. Description
This project implements a **RAM module with synchronous write and asynchronous read**:

- ✍️ **WRITE** happens only on the **posedge of `clk`**
- 📖 **READ** happens **immediately** based on `addr` (no clock needed)

The design is **parameterized** for data width and memory depth and includes a reset that clears all memory locations.

---

## ⭐ 3. Key Features
- ⚙️ Parameterized design:
  - `WIDTH` = data width (default **8-bit**)
  - `DEPTH` = number of locations (default **16**)
  - `ADDR = $clog2(DEPTH)` = address width
- ✍️ **Synchronous write** (clocked)
- 📖 **Asynchronous read** (combinational)
- 🔄 Reset clears memory to all zeros

---

## 🔌 4. Module Interface

### 📄 Module Declaration
```verilog
module Asy_RAM(clk,rst,w_r,wdata_in,rdata_out,addr);
```

### 🧾 Ports
| Signal      | Dir | Width | Description |
|------------|-----|-------|-------------|
| `clk`      | In  | 1     | ⏱️ Clock (used for write) |
| `rst`      | In  | 1     | 🔄 Reset (clears memory) |
| `w_r`      | In  | 1     | ✍️ Write enable (1 = write, 0 = no write) |
| `addr`     | In  | `ADDR` | 📍 Memory address |
| `wdata_in` | In  | `WIDTH` | 📨 Write data |
| `rdata_out`| Out | `WIDTH` | 📤 Read data (asynchronous) |

---

## 🧠 5. Functional Behavior

### 🔄 Reset
On rising edge of `clk` when `rst = 1`:
- 🧼 All memory locations are cleared:
  - `mem[i] <= 0`

### ✍️ Write Operation (Synchronous)
On rising edge of `clk` when:
- `rst = 0` and `w_r = 1` ✅  
Then:
- `mem[addr] <= wdata_in`

### 📖 Read Operation (Asynchronous)
Read data is always available without waiting for the clock:
```verilog
assign rdata_out = mem[addr];
```
✅ Any change in `addr` updates `rdata_out` immediately (combinationally).

---

## ⚙️ 6. Parameter Example
To build a **64x16 RAM**:
```verilog
Asy_RAM #(
  .DEPTH(64),
  .WIDTH(16)
) u_ram (
  .clk(clk),
  .rst(rst),
  .w_r(w_r),
  .wdata_in(wdata_in),
  .rdata_out(rdata_out),
  .addr(addr)
);
```

---

## 📁 7. Files
- 📄 `Asy_RAM.v` — Async-read RAM module (sync write + async read)

