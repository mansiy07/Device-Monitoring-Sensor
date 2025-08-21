# Device-Monitoring-Sensor
📌Feature: 
Simulates multiple devices with random CPU usage, temperature, and battery level
Detects critical conditions (like overheating, high CPU load, or low battery)
Logs results in a file (device_logs.txt)
Can monitor multiple devices in one run

Advantages of This Project
1. Practical Use Case – Simulates how IoT monitoring works in real-world devices.
2. Core Java Only – No external libraries, easy to understand.
3. Scalable – Can add more sensors (network speed, memory usage).
4. File Logging – Keeps history for analysis, just like real monitoring systems.
5. Good for Learning OOP – Uses classes, methods, encapsulation, and file handling.

🖥️Source Code:
import java.io.FileWriter;
import java.io.IOException;
import java.time.LocalDateTime;
import java.util.Random;
import java.util.Scanner;

// Represents a device with sensors
class Device {
    private String deviceName;
    private int cpuUsage;
    private int temperature;
    private int batteryLevel;

    public Device(String deviceName) {
        this.deviceName = deviceName;
        Random rand = new Random();
        this.cpuUsage = rand.nextInt(101);      // 0-100%
        this.temperature = rand.nextInt(101);   // 0-100 °C
        this.batteryLevel = rand.nextInt(101);  // 0-100%
    }

    public String getDeviceName() {
        return deviceName;
    }

    public String checkStatus() {
        StringBuilder status = new StringBuilder("Device: " + deviceName + " -> ");

        if (cpuUsage > 85) {
            status.append("⚠ High CPU Usage (" + cpuUsage + "%). ");
        }
        if (temperature > 70) {
            status.append("🔥 Overheating (" + temperature + "°C). ");
        }
        if (batteryLevel < 20) {
            status.append("🔋 Low Battery (" + batteryLevel + "%). ");
        }
        if (status.toString().endsWith("-> ")) {
            status.append("✅ Device Normal.");
        }
        return status.toString();
    }
}

public class DeviceMonitoringSystem {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter number of devices to monitor: ");
        int n = sc.nextInt();

        for (int i = 1; i <= n; i++) {
            Device d = new Device("Device-" + i);
            String result = d.checkStatus();
            System.out.println(result);
            saveLogs(result);
        }
        sc.close();
    }

    // Save logs in file
    private static void saveLogs(String log) {
        try (FileWriter writer = new FileWriter("device_logs.txt", true)) {
            writer.write(LocalDateTime.now() + " | " + log + "\n");
        } catch (IOException e) {
            System.out.println("❌ Error saving logs.");
        }
    }
}
