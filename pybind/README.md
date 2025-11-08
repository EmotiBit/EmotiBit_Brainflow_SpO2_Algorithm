# Running EmotiBit Brainflow SpO2 Algorithm using `pybind`
## Requirements
- Python
- Python virtual environment (refer to the Emotibit Plugins [repo](https://github.com/EmotiBit/EmotiBit_Plugins/blob/main/README_py.md#Creating-python-env-with-pybind11) for specific instructions on how to setup)
- CMake
- [EmotiBit ArduinoFilters](https://github.com/EmotiBit/EmotiBit_ArduinoFilters)
  - The EmotiBit ArduinoFilter directory should be on the same level as this repository.
  - **Option 1:** Library Manager 
    1. Open Arduino IDE, 
    1. open the Library Manager
    1. Download EmotiBit_ArduinoFilters `v1.0.1` (if not already installed)
  - **Option 2:** `git clone` (*example showing cloning in Arduino Libraries directory, but user can clone in any other location, as long as ArduinoFilters is also cloned at the same directory level*)
    1. `cd` into Arduino libraries folder
       - **Windows:** `C:\Users\{username}\Documents\Arduino\libraries`
       - **Mac:** `/Users/{username}/Documents/Arduino/libraries`
       - **Linux:** `/home/{username}/Arduino/libraries`
    1. Run `git clone https://github.com/EmotiBit/EmotiBit_ArduinoFilters`

## Build
- Open a terminal window
- `cd` into the `pybind` directory
- Before being able to run cmake, you will need to update the path to `pybind11` if required. `pybind11` is installed in the python virtual environment, as the link in the previous section explains.
  - We tested this with cirtual environment created using python 3.12, and therefore the path to pybind in the current CMakeLists.txt file is `py-env/lib/python3.12/site-packages/pybind11/share/cmake/pybind11/`.
- Run `cmake -B build` to generate the build files (the build files will be under a directory named `build`)
- Run `cmake --build build --config Release` to generate the `.pyd` file
  - On Windows using MSVC, the `.pyd` file will be under `pybind/build/Release`
  - Otherwise, the `.pyd` file will be under `pybind/build`
- Activate the Python virtual environment (instructions found in Emotibit Plugins [repo](https://github.com/EmotiBit/EmotiBit_Plugins/blob/main/README_py.md#setting-up-python-virtual-environment))

### Running run.py
- Open `run.py` and uncomment lines 6-7 (if on linux/macOS), or uncomment lines 10-11 (if on Windows)
- `run.py` runs on emotibit data. We have checked in test data in thie repositoty. By default, the script looks for a `data` directory on the same level. Therefore, to test the script, you can extract the `tests/sit-stand-sit_v0.0.0/ip900ap/sit-stand-sit_v0.0.0_ip900ap.zip` file and place the EMotiBit.csv file in the `data` folder. You can then use the EmotiBit data parser to parse the files, since the script looks for files ending in `_PR` and `_PI`.

## Testing
- Unzip the `.zip` file in `tests/simulated-unobstructed-airway_v0.0.0/ip900ap`
- Run the EmotiBit Data Parser on `tests/simulated-unobstructed-airway_v0.0.0/ip900ap/EmotiBit.csv`
- Verify that `EmotiBit_PI.csv`, `EmotiBit_PR.csv`, and `EmotiBit_UN.csv` exist under `tests/simulated-unobstructed-airway_v0.0.0/ip900ap`
- Switch to the terminal window in the `pybind` directory
- Run `python run.py ../tests/simulated-unobstructed-airway_v0.0.0/ip900ap`
  - `matplotlib` will need to be installed for the script to run successfully
- A plot of the raw PPG data overlayed with the calculated SpO2 should appear
**Note:** The timestamp used by `run.py` is the `EmotiBitTimestamp` column in the Emotibit `.csv` files
