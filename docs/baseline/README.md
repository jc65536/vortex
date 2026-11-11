
# Vortex v3.0 — SimX Baseline

CS254A Project #3: Out-of-Order Execution

## 1. Environment

- Ubuntu 22.04 (WSL2)
- GCC 11.4.0
- Vortex v3.0
- SimX (RV32)

## 2. Setup

Clone Vortex v3.0:

```bash
git clone --branch v3.0 --recursive https://github.com/vortexgpgpu/vortex.git
cd vortex
```

Install dependencies:

```bash
sudo apt update
sudo apt install -y build-essential cmake ccache \
  zlib1g-dev libtinfo-dev libncurses-dev uuid-dev \
  libboost-serialization-dev libpng-dev libhwloc-dev
```

Configure and build:

```bash
mkdir -p build
cd build
../configure --xlen=32 --tooldir=$HOME/tools
./ci/toolchain_install.sh
make -s -j4
make install
```

## 3. Run Baseline

From the `build/` directory:

```bash
./ci/blackbox.sh --driver=simx --app=vecadd --perf=1 \
  2>&1 | tee baseline_vecadd.log
```

## 4. Results

Test: vecadd (n=64, buf=256B)

Status: PASSED

| Metric | Result |
|---|---:|
| Instructions | 400 |
| Cycles | 1270 |
| IPC | 0.315 |
| Scoreboard Stalls | 33% |
| Scheduler Idle | 64% |
| Warp Occupancy | 97% |
| Fetch Stalls | 4% |
| Load Latency | 22.46 cycles |

## 5. Next Steps

1. Create dependency microbenchmarks.
2. Implement a 4-entry OoO issue window.
3. Verify correctness.
4. Compare OoO performance against baseline.

Full output: `vecadd.log`
