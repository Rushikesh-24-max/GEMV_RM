# General Matrix-Vector Multiply (GEMV) Auto-Tuning

### Colab Execution Commands

1. **Navigate to the GEMV kernel directory**:
   ```bash
   cd kernels/gemv/
   ```

2. **Generate configurations (Latin Hypercube Sampling)**:
   ```bash
   python gen_configs.py
   ```
   * Outputs 7500 configurations to `gemv_configurations.csv`.

3. **Run Full Latin Hypercube Auto-Tuning**:
   ```bash
   python main.py
   ```
   * Evaluates configs in chunks of 500. Adjust `start_idx` for resuming.
   * Outputs dataset to `gemv_tuning_dataset_1.csv`.

4. **Manual Single Compilation & Run (Sanity Check)**:
   ```bash
   # Compile with specific tuning parameters
   nvcc -O3 main.cu -arch=sm_75 -o gemv_autotune \
     -DBLOCK_DIM_X=256 -DROWS_PER_BLOCK=4 \
     -DTILE_K=32 -DUNROLL_FACTOR=4

   # Run with M=1024, K=1024
   ./gemv_autotune 1024 1024
   ```
