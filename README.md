# [PiAxe-Miner](https://github.com/shufps/piaxe-miner) Installation Guide for Raspberry Pi

## Prerequisites
- Raspberry Pi (or similar device) running Linux
- Git installed (`sudo apt-get install git`)
- Build essentials installed (`sudo apt-get install build-essential`)
- Python3 and pip3 installed
- Internet connection

## Installation Steps

1. Install required dependencies:
```bash
sudo apt-get update
sudo apt-get install -y cmake libssl-dev build-essential git screen python3 python3-pip nano
```

2. Clone the repository:
```bash
git clone https://github.com/shufps/piaxe-miner.git
cd piaxe-miner
```

3. Install Python requirements:
```bash
pip3 install -r requirements.txt --break-system-packages
```

4. Build the project:
```bash
mkdir build
cd build
cmake ..
make -j4
cd ..
```

5. Configure the miner:
```bash
cp config.example.yml config.yml
nano config.yml
```

Nano Editor Commands for config.yml:
- Navigate with arrow keys
- Find text: `Ctrl + W`
- Save changes: `Ctrl + O`, then `Enter`
- Exit nano: `Ctrl + X`

Key things to edit in config.yml:
- Set `solo: true`
- Find your miner type (e.g., qaxe+, piaxe)
- Remove the # from in front of your miner type
- Add # in front of other miner types
- Configure correct USB serial ports

For PiAxe devices:
```yaml
piaxe:
  serial_port: "/dev/ttyS0"
  # other settings...
```

For QAxe devices:
```yaml
qaxe:
  serial_port_asic: "/dev/ttyACM0"
  serial_port_ctrl: "/dev/ttyACM1"
  # other settings...
```

To find the correct USB serial ports:
```bash
dmesg | grep tty
```

Example complete config for QAxe+ miner:
```yaml
solo: true
miner: qaxe+
qaxe+:
  name: QAxe+
  chips: 4
  fan_speed_1: 1.0
  fan_speed_2: 1.0
  asic_frequency: 490
  extranonce2_interval: 1.5
  serial_port_asic: "/dev/ttyACM0"
  serial_port_ctrl: "/dev/ttyACM1"
```

6. Configure mining address and pool:
```bash
cp start_mainnet_publicpool_example.sh start_mainnet.sh
nano start_mainnet.sh
```

Nano Editor Commands for start_mainnet.sh:
- Navigate to the wallet address and pool settings
- Replace the example wallet address with yours
- Update the stratum URL if needed
- Save: `Ctrl + O`, then `Enter`
- Exit: `Ctrl + X`

7. Make the script executable:
```bash
chmod +x start_mainnet.sh
```

8. Start the miner in a screen session:
```bash
# Create a new screen session named "piaxe"
screen -S piaxe

# Start the miner
./start_mainnet.sh
```

9. Detach from the screen session:
- Press `Ctrl + A`
- Then press `D`
- You can now safely disconnect from SSH while the miner continues running

## Screen Management Commands

- Reattach to the mining session:
```bash
screen -r piaxe
```

- List all screen sessions:
```bash
screen -ls
```

- Kill the mining screen session (if needed):
```bash
screen -X -S piaxe quit
```

## Monitoring

While attached to the screen session, you can view the mining output directly. You can also check the logs:
```bash
tail -f miner.log
```

## Troubleshooting

If you have issues with USB serial ports:
1. Use `dmesg | grep tty` to see all available serial ports
2. Check if your device is properly connected
3. Make sure you have the correct permissions to access the serial ports:
   ```bash
   sudo usermod -a -G dialout $USER
   # Log out and back in for changes to take effect
   ```
4. Verify the serial port settings in config.yml match your actual device ports

Basic nano editor commands for any file edits:
- `Ctrl + O`: Save file
- `Ctrl + X`: Exit nano
- `Ctrl + W`: Search for text
- `Ctrl + K`: Cut line
- `Ctrl + U`: Paste line
- Arrow keys: Navigate
- `Page Up`/`Page Down`: Scroll through file



## Warning
This guide doesn't claim to be complete or a concrete tutorial, it shall just help you with the absolute basics. ALWAYS do your own research, start in the [PiAxe-Miner](https://github.com/shufps/piaxe-miner) repository.
