# simple-sunspec
Python-routines to read data from SunSpec-compliant PV-inverters (e.g. SolarEdge, SMA, Fronius and others)

If you want a full-fledged SunSpec library for Python, have a look at [pysunspec2](https://github.com/sunspec/pysunspec2). If you want a Python-library and commandline-tool that can directly be used for reading data and returning it as JSON or storing it in an Influx database, have a look at [solaredge_modbus](https://github.com/nmakel/solaredge_modbus).

The routines in this repository can retrieve basic SunSpec-data over modbus-TCP or modbus-RTU, but they can also be used to access inverters remotely through the MQTT-modbus-gateway provided by Teltonika in its LTE-routers, such as the [RUT240](https://wiki.teltonika-networks.com/view/RUT240_Modbus#MQTT_Gateway).

The functions `sunspec_get_static()` and `sunspec_get_vars()` in `sunspec_example.py` can be used to retrieve a dictionary of respectively static and dynamic variables from a PV-inverter that supports SunSpec and has Modbus TCP enabled. SunSpec-compatible inverters include nearly all models from SolarEdge and Fronius, as well as newer models from SMA, Solis, Goodwe, Huawei, Solax, Delta and a number of other manufacturers.

The example code in `sunspec_example.py` requires a simple class that provides the actual functions to connect to a modbus device and to read and write register values. The example uses the Modbus TCP client in `modbus_device_tcp.py`, which is based on [Pymodbus 2.x](https://pymodbus.readthedocs.io/en/v2.5.3/index.html). This has been tested with an older SolarEdge SE3600 inverter and Pymodbus [2.1.0](https://pymodbus.readthedocs.io/en/v2.1.0/), which is available in the repositories of Ubuntu 22.04 LTS, as the package `python3-pymodbus`. A Modbus TCP class for [Pymodbus 3.x](https://pymodbus.readthedocs.io/en) is also provided, in `modbus_device_tcp_pymodbus3.py`, but this has not yet been tested. It is relatively easy to add alternative classes for other libraries, for Modbus RTU or for other access methods (e.g. modbus over a network tunnel, or using a modbus gateway).

Most inverters do not have SunSpec over Modbus enabled by default, for security reasons. For SolarEdge inverters, you can consult the current [Technical Note on SunSpec Logging](https://knowledge-center.solaredge.com/sites/kc/files/sunspec-implementation-technical-note.pdf) for more information. On older inverter models with an LCD display, you can generally enable Modbus TCP in the inverter menu, under: *Communication -> LAN Conf -> Modbus TCP*. The default password for the configuration menu is `12312312` (but your installer may have changed this). If you use a newer SolarEdge HDWave inverter with SetApp, things may be a bit more complicated. If you have full access to the system settings in SetApp (e.g. if you're an installer), you can enable SunSpec-access over Modbus TCP in SetApp under *Site Communication -> Modbus TCP -> Enable*. If you do not have access to the system settings in SetApp (e.g. if you had a company instal the system for you), you may have to ask your installer to enable Modbus TCP for you. Maybe you can do this yourself if your installer is willing to [increase the access level](https://www.youtube.com/watch?v=Aph3pRSJ--I) of your SolarEdge system owner account. [Setting the role](https://knowledge-center.solaredge.com/sites/kc/files/se-monitoring-portal-site-admin.pdf) of your owner account to *Full Access* might give you sufficient rights to enable Modbus, but I'm not sure. Alternativey, you can try registering with SolarEdge as a self-installer and ask your installer to assign your inverter to your account.

