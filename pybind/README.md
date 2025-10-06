# Running EmotiBit Brainflow SpO2 Algorithm using `pybind`
## Requirements
- Python
- Python virtual environment (refer to the Emotibit Plugins [repo](https://github.com/EmotiBit/EmotiBit_Plugins/blob/main/README_py.md#setting-up-python-virtual-environment) for specific instructions on how to setup)
- CMake
- [EmotiBit ArduinoFilters](https://github.com/EmotiBit/EmotiBit_ArduinoFilters)
  - **Library Manager:** 
    1. Open Arduino IDE, 
    1. open the Library Manager
    1. Download EmotiBit_ArduinoFilters `v1.0.1` (if not already installed)
  - **`git clone`:**
    1. `cd` into Arduino libraries folder
       - **Windows:** `C:\Users\{username}\Documents\Arduino\libraries`
       - **Mac:** `/Users/{username}/Documents/Arduino/libraries`
       - **Linux:** `/home/{username}/Arduino/libraries`
    1. Run `git clone https://github.com/EmotiBit/EmotiBit_ArduinoFilters`
    1. Run `git checkout 2a1d6cfa0b36ba8cf9e9a19b97d8fa261519a2d9`

## Build
- Open a terminal window
- `cd` into the `pybind` directory
- Activate the Python virtual environment (instructions found in Emotibit Plugins [repo](https://github.com/EmotiBit/EmotiBit_Plugins/blob/main/README_py.md#setting-up-python-virtual-environment))
- Run `cmake -B build` to generate the build files (the build files will be under a directory named `build`)
- Run `cmake --build build --config Release` to generate the `.pyd` file
  - On Windows using MSVC, the `.pyd` file will be under `pybind/build/Release`
  - Otherwise, the `.pyd` file will be under `pybind/build`
- Open `run.py` and uncomment lines 6-7 (if on linux/macOS), or uncomment lines 10-11 (if on Windows)

## Testing
- Run the EmotiBit Data Parser on `tests/simulated-unobstructed-airway_v0.0.0/ip900ap/EmotiBit.csv`
- Verify that `EmotiBit_PI.csv`, `EmotiBit_PR.csv`, and `EmotiBit_UN.csv` exist under `tests/simulated-unobstructed-airway_v0.0.0/ip900ap`
- Switch to the terminal window in the `pybind` directory
- Run `python run.py ../tests/simulated-unobstructed-airway_v0.0.0/ip900ap`
  - `matplotlib` will need to be installed for the script to run successfully
- A plot of the raw PPG data overlayed with the calculated SpO2 should appear
