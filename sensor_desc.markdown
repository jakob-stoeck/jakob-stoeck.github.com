---
title: Literature review for standards in sensor descriptions
---

## Literature review for standards in sensor descriptions

Those are my notes while finding an industry-standard for the description of sensors and sensor data.

### SensorML

process/provider-centric

> The ability to easily discover, access, and process sensors existing within different communities is very rare. SensorML provides a common framework for describing virtually any sensor system, as well as the processing that might be associated with these sensor systems. [SML]

* Detectors take one input and generate one or more output values. In SensorML "processes" if they are not connected to a spatial or temporal domain, "components" otherwise.
* Sensors are several processes linked together. They are called "process chains" with no spatial or temporal domain, "system" otherwise. They use the Composite Design pattern [PAT1995], such that a process chain itself is a process.

#### Data Types (SWE)

* primitive data types, complementing those implemented in GML
* general purpose aggregate data types, including records, arrays, vectors and
*atrices
* aggregate data types with specialized semantics, including position, curve, and time-aggregates
* standard encodings to add semantics, quality indication and constraints to both primitive and aggregate types
* specialized components to support semantic definitions, as required above
* a notation for the description of XML and non-XML array encodings.

plus positional and temporal data.

#### Encoding

* Text
* Binary
* MIME type
* I/O Stream
* XML Block

#### Example of a simple data value

```xml
<swe:Quantity gml:id="elevationAngle" fixed="false" definition="urn:ogc:def:property:OGC:scanAngle">
	<uom code="deg"/>
	<swe:constraint>
		<swe:AllowedValue id="elevationAngleRange">
			<swe:interval> 10.0 45.2 </swe:interval>
		</swe:AllowedValue >
	</swe:constraint>
	<swe:quality>
		<swe:QuantityRange definition=" urn:ogc:def:property:OGC:tolerance2std">
			<swe:value> -0.02 0.02 </value>
		</swe:QuantityRange>
	</swe:quality>
</swe:Quantity>
```

#### Example of a system

```xml
<!--~~~~~~~~~~~~~~~~~~~~~~~~ -->
<!-- System Interface -->
<!-- ~~~~~~~~~~~~~~~~~~~~~~~ -->
<interfaces>
	<InterfaceList>
		<interface name="RS-232">
			<InterfaceDefinition>
				<!-- http://www.interfacebus.com/Design_Connector_RS232.html -->
				<applicationLayer>
					<swe:Category definition="urn:ogc:def:property:OGC:applicationLink">
						<swe:value>urn:davis:def:protocol:weatherLink</swe:value>
					</swe:Category>
				</applicationLayer>
				<physicalLayer>
					<swe:DataRecord definition="urn:ogc:def:property:OGC:RS232">
						<swe:field name="num-bits">
							<swe:Count definition="urn:ogc:def:property:OGC:numberOfBits">
								<swe:value>8</swe:value>
							</swe:Count>
						</swe:field>
						<swe:field name="parity">
							<swe:Boolean definition="urn:ogc:def:property:OGC:parity">
								<swe:value>false</swe:value>
							</swe:Boolean>
						</swe:field>
					</swe:DataRecord>
				</physicalLayer>
				<mechanicalLayer>
					<swe:DataRecord definition="urn:ogc:def:property:OGC:DB9">
						<swe:field name="pin-out">
							<swe:Category definition="urn:ogc:def: property:OGC:pinout">
								<swe:value>EIA574</swe:value>
							</swe:Category>
						</swe:field>
					</swe:DataRecord>
				</mechanicalLayer>
			</InterfaceDefinition>
		</interface>
	</InterfaceList>
</interfaces>
<!--~~~~~~~~~~~~~-->
<!--System Inputs-->
<!--~~~~~~~~~~~~~-->
<inputs>
	<InputList>
		<input name="atmosphericTemperature">
			<swe:ObservableProperty definition="urn:ogc:def:property:OGC:temperature"/>
		</input>
		<input name="wind">
			<swe:ObservableProperty definition="urn:ogcdef:property:OGC:windSpeed"/>
		</input>
		<input name="rainfall">
			<swe:ObservableProperty definition="urn:ogc:def:property:OGC:rainfall"/>
		</input>
		<input name="atmosphericPressure">
			<swe:ObservableProperty definition="urn:ogcdef:property:OGC:pressure"/>
		</input>
	</InputList>
</inputs>
<!--~~~~~~~~~~~~~~-->
<!--System Outputs-->
<!--~~~~~~~~~~~~~~-->
<outputs>
	<OutputList>
		<output name="weatherMeasurements">
			<swe:DataRecord gml:id="outputGroup">
				<swe:field name="time">
					<swe:Time definition="urn:ogc:def:property:OGC:observationTime">
						<swe:uom xlink:href="urn:ogc:def:unit:ISO:8601"/>
					</swe:Time>
				</swe:field>
				<swe:field name="temperature">
					<swe:Quantity definition="urn:ogc:def:property:OGC:temperature">
						<swe:uom code="cel"/>
					</swe:Quantity>
				</swe:field>
				<swe:field name="windSpeed">
					<swe:Quantity definition="urn:ogc:def:property:OGC:windSpeed">
						<swe:uom code="m/s"/>
					</swe:Quantity>
				</swe:field>
				<swe:field name="windDirection">
					<swe:Quantity definition="urn:ogc:def:property:OGC:windDirection">
						<swe:uom code="deg"/>
					</swe:Quantity>
				</swe:field>
				<swe:field name="rainfall">
					<swe:Quantity definition="urn:ogc:def:property:OGC:rainfall">
						<swe:uom code="mm"/>
					</swe:Quantity>
				</swe:field>
				<swe:field name="barometricPressure">
					<swe:Quantity definition="urn:ogc:def:property:OGC:barometricPressure">
						<swe:uom code="hPa"/>
					</swe:Quantity>
				</swe:field>
				<swe:field name="windChill">
					<swe:Quantity definition="urn:ogc:def:property:OGC:windChill">
						<swe:uom code="cel"/>
					</swe:Quantity>
				</swe:field>
			</swe:DataRecord>
		</output>
		<output name="minMax">
			<swe:DataRecord gml:id="minMaxTemperatures">
				<swe:field name="dailyMinTemp">
					<swe:Quantitydefinition="urn:ogc:def:property:OGC:dailyMinimumTemperature"/>
				</swe:field>
				<swe:field name="dailyMaxTemp">
					<swe:Quantitydefinition="urn:ogc:def:property:OGC:dailyMinimumTemperature"/>
				</swe:field>
			</swe:DataRecord>
		</output>
	</OutputList>
</outputs>
<components>
	<ComponentList>
		<!--~~~~~~~~~~~~~~~-->
		<!-- Clock -->
		<!--~~~~~~~~~~~~~~~-->
		<component name="clock" xlink:arcrole="urn:ogc:def:process:OGC:detector"xlink:href="urn:ogc:sensor:Davis:clock"/>
		<!--~~~~~~~~~~~~~~~-->
		<!-- Barometric -->
		<!--~~~~~~~~~~~~~~~-->
		<component name="barometer" xlink:arcrole ="urn:ogc:def:process:OGC:detector"xlink:href="urn:ogc:sensor:Davis:7441"/>
		<!--~~~~~~~~~~~~~~~-->
		<!-- Thermometer -->
		<!--~~~~~~~~~~~~~~~-->
		<component name="thermometer" xlink:arcrole="urn:ogc:def:process:OGC:detector"xlink:href="urn:ogc:sensor:Davis:7817"/>
		<!--~~~~~~~~~~~~~~~-->
		<!-- Anemometer -->
		<!--~~~~~~~~~~~~~~~-->
		<component name="windSpeedTransducer" xlink:arcrole="urn:ogc:def:process:OGC:detector"xlink:href="urn:ogc:sensor:Davis:7911Spd"/>
		<component name="windDirectionTransducer" xlink:role="urn:ogc:def:process:OGC:detector"xlink:href="urn:ogc:sensor:Davis:7911Dir"/>
		<!--~~~~~~~~~~~~~~~-->
		<!-- Rain Gauge -->
		<!--~~~~~~~~~~~~~~~-->
		<component name="rainGaugeSensor" xlink:role="urn:ogc:def:process:OGC:detector"xlink:href="urn:ogc:sensor:Davis:7852M"/>
		<!--~~~~~~~~~~~~~~~-->
		<!-- Wind Chill -->
		<!--~~~~~~~~~~~~~~~-->
		<component name="windChill" xlink:role="urn:ogc:def:process:OGC:process"xlink:href="urn:ogc:process:windChill_v01"/>
	</ComponentList>
</components>
```

#### XML Schemas for SensorML

* base.xsd
* process.xsd
* method.xsd
* system.xsd
* sensorML.xsd

#### Further Information

* http://www.opengeospatial.org/standards/sensorml
* http://www.sensorsmag.com/networking-communications/a-sensor-model-language-moving-sensor-data-internet-967
* [SML] OpenGIS Sensor Model Language (SensorML) http://www.opengeospatial.org/standards/sensorml
* [PAT1995] Gamma, E., Helm, R., Johnson, R., Vlissides, J. Design Patterns: Elements of Reusable Object-Oriented Software. 395pp. Addison Wesley, 1995.

### TransducerML

data-centric

Purpose

* Interoperability and fusion of disparate sensor data: TML enables the interoperability of heterogeneous sensor systems by providing a self describing data exchange protocol based on XML. This enables the fusion of multi-sensor, multi-source data into a common operating picture.
* Data exchange across multiple sensor types:  TML is application independent, therefore well suited to address sensor data exchange across multiple operational domains.
* Registration of different sensors and the correlations between them: TML maintains relative and absolute time sequencing of data from various (or all as required) sensors within a system. TML enables the analysts to compare temporal and spatially similar collected data and or compare disparate temporal or spatially collected data thus providing multiple domain coupling.
* One common processor handles all incoming sensor data: TML as a common sensor model and format facilitates the development of a common processor and improves multi-sensor/type data fusion.  This enables each sensor to have a common memory structure.
* Faster and more accurate targeting: TML provides high fidelity exchange of sensor data and facilitates precision space-time registration as well as error characterization of data.
* Plug and play sensor: TML promotes plug-n-play sensors and is therefore adaptable to new sensors because no modification is required to the TML enabled processor.
* Preservation of raw sensor data: Through time-tagging TML allows precise ordering of raw sensor data and the reconstruction of raw data streams.  Moreover, this ability ensures data can be smartly archived and retrieved without being corrupted or having to search through volumes of data. The operator can easily set up a search engine to “find” specific data either temporally or spatially.
* Sensor discovery: TML enables a user to obtain a list of available sensors on a real-time basis, and to select specific sensors from which to receive data.
* Bi-directional Control: TML seamlessly allows control of transducers in the same data stream as sensor data.  Complex sensors with included control systems such as image positioning, camera shutter, local alarms, etc. are controlled from the same client application.
* Small Highly Efficient Footprint: TML, by design, has a very low overhead structure which allows for much higher bandwidth use.  Very high rate clocks can be used to time stamp data.
* Sensor and Transducer Modeling: A large array of modeling parameters are included in TML.  Each sensor or transducer can be characterized with a full range of parameters including transfer functions, calibration data, unit step function, etc. [OCG]

* [OCG] Transducer Markup Language http://www.ogcnetwork.net/node/105

Transducer is a receiver and/or transmitter

#### Example

```xml
<tml>
	<system name="SYS01">
		<sysClk>
			<period>
				<values>0.001</values>
			</period>
		</sysClk>
		<transducers>
			<transducer name="SYS01_CAMERA">
				<logicalDataModel>
					<dataSet id="CAMERA_IMAGE">
					</dataSet>
				</logicalDataModel>
			</transducer>
		</transducers>
		<process name=”PROCESS_1” uid=”SYS01_PCS”>
			<input>
				<procDataSet name="CAMERA_IMAGE">
				</dataSet>
			</input>
		</process>
		<relations>
			<posRelation UidRef="SYS01_CAMERA" refSystem="SYS01_GPS"/>
				<spatialCoord coordName="latitude">
					<values bindUid="SYS01_GPS_LAT"/>
				</spatialCoord>
			</posRelation>
		</relations>
		<clusterDesc name="SYS01_GPS_LLA">
			<dataUnitEncoding>
			</dataUnitEncoding>
		</clusterDesc>
	</system>
	<!--start data stream-->
	<data clk="263084829" ref="SYS01_TIMESTAMP">2006-03-02T14:39:41.04Z</data>
	<data clk="263085859" ref="SYS01_GPS_LLA">0.577041 -2.035342 0.000000</data>
	<data clk="12344" ref="SYS01_CAMERA_RGB24_BASE64">ABabdks2836875[…]ARBKDK==</data>
	<!--continue data stream-->
</tml>
```

### Additional Plus: Semantic Sensor Web

> Sensors around the globe currently collect avalanches of data about the world. The rapid development and deployment of sensor technology is intensifying the existing problem of too much data and not enough knowledge[citation needed]. With a view to alleviating this glut, sensor data can be annotated with semantic metadata to increase interoperability between heterogeneous sensor networks, as well as to provide contextual information essential for situation awareness. Semantic web techniques can greatly help with the problem of data integration and discovery as it helps map between different metadata schema in a structured way. […]
> Additionally, real-time extension of the Semantic Sensor Web concept is being developed, called Sensor Wiki. The motivation behind this concept is to allow real-time browsing of the physical world consistent with the STT situational awareness goal. Understanding the physical world via a myriad of sensors is now possible. [wiki1]

[wiki1]: http://en.wikipedia.org/wiki/Semantic_Sensor_Web "Semantic Sensor Web"

### Observation and Measurements (O&M)

ISO/PRF 19156

schema encoding for observations -> user-centric

O&M defines a core set of properties for an observation:

* feature of interest
* observed property
* result
* procedure - the instrument, algorithm or process used (which may be described using SensorML)
* phenomenon time - the real-world time associated with the result
* result time - the time when the result was generated
* valid time - the period during which the result may be used

Sampling Features

* sampled feature - which links the sampling artefact with the real-world feature of interest
* related observation
* related sampling feature - linking sampling features into complexes

#### Example of an observation

```xml
<om:OM_Observation xmlns:om="http://www.opengis.net/om/2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:gml="http://www.opengis.net/gml/3.2" gml:id="shapeTest2" xsi:schemaLocation="http://www.opengis.net/om/2.0 http://schemas.opengis.net/om/2.0/observation.xsd">
	<gml:description>Observation test instance</gml:description>
	<gml:name>Shape observation test</gml:name>
	<om:type xlink:href="http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_TemporalObservation"/>
	<om:phenomenonTime>
		<gml:TimeInstant gml:id="ot2t">
			<gml:timePosition>2005-01-11T17:22:25.00</gml:timePosition>
		</gml:TimeInstant>
	</om:phenomenonTime>
	<om:resultTime>
		<gml:TimeInstant gml:id="ot2ts">
			<gml:timePosition>2005-01-12T09:25:00.00</gml:timePosition>
		</gml:TimeInstant>
	</om:resultTime>
	<om:procedure xlink:href=" http://www.example.org/register/process/stopwatch"/>
	<om:observedProperty xlink:href="http://www.w3.org/2006/time#DurationDescription"/>
	<om:featureOfInterest xlink:title="World Championship Cycling Women's 500 m sprint"/>
	<om:result xsi:type="gml:TimePrimitivePropertyType">
		<gml:TimePeriod gml:id="tm876">
			<gml:beginPosition>2010-03-24</gml:beginPosition>
			<gml:endPosition indeterminatePosition="after"/>
			<gml:duration>PT33.381S</gml:duration>
		</gml:TimePeriod>
	</om:result>
</om:OM_Observation>
```

https://www.seegrid.csiro.au/wiki/AppSchemas/ObservationsAndSampling
http://en.wikipedia.org/wiki/Observations_and_Measurements
http://www.geogr.uni-jena.de/fileadmin/Geoinformatik/Lehre/Diplomarbeiten/DA_Andres.pdf

### IEEE 1451

The IEEE (Institute of Electrical and Electronics Engineers) 1451 smart transducer interface standards provide the common interface and enabling technology for the connectivity of transducers to microprocessors, control and field networks, and data acquisisiton and instrumentation systems. The standardized TEDS specified by IEEE 1451.2 allows the self-description of sensors and the interfaces provide a standardized mechanism to facilitate the plug and play of sensors to networks. The network-independent smart transducer object model defined by IEEE 1451.1 allows sensor manufacturers to support multiple networks and protocols. Thus, transducer-to-network interoperabily is on the horizon. The inclusion of P1451.3 and P1451.4 to the family of 1451 standars will meet the needs of the analog transducer users for high-speed applications. [LEE00]
                     
One of the key elements of the IEEE 1451 (Dot 0) standards is the definition of Transducer Electronic Data Sheet (TEDS) for each transducer. TEDS can, however, be implemented in two ways. First, the TEDS can reside in embedded memory, typically an EEPROM, within the transducer itself which is connected to the measurement instrument or control system. Second, a virtual TEDS can exist as a data file accessible by the measurement instrument or control system.

The memory contents are divided into four areas

* Area 1: An internationally unique identification number (cannot be changed).
* Area 2: The base area (basic TEDS) to the configuration defined in standard IEEE 1451.4. This contains the transducer type, manufacturer and serial number of the transducer (writing to or amending this area requires user rights for the ID level).
* Area 3: This area contains characteristic transducer data that can be defined by the manufacturer, by calibration or by the user. They are organized into so-called templates which store defined groups of data in a summarized form.
At least 1 template must be present for transducer description; this contains the specification

 * the transducer type,
 * the measured quantity,
 * the electrical output signal,
 * the required excitation,
 * the characteristic curve, etc.

The standard specifies templates for the most important types of transducers. [HBS]

#### Data Types

* UNINT:￼Unsigned integer
* Chr5:￼5-bit character
* ASCII:￼Standard 7-bit ASCII
* Date:￼Number of days since January 1, 1998
* Single:￼Single-precision floating point
* ConRes:￼Constant resolution. This is a custom data type for compressed floating point values that provides a linear mapping of a defined interval
* ConRelRes:￼Constant relative resolution. This is a custom data type for compressed floating point values that provides a logarithmic mapping of a defined interval
* Enumeration:￼References a defined enumerated data type defined in the template

#### Example Template 25: Accelerometer and Force Transducers

This template is intended for use with dynamic accelerometers and force transducers which are constant-current powered, or IEPE. (Excerpt) [NAT]

<table>
<tr>
	<th>Select</th>
	<th>Select</th>
	<th>Property/Command</th>
	<th>Description</th>
	<th>Access</th>
	<th>Bits</th>
	<th>Data Type (and Range)</th>
	<th>Units</th>
</tr>
<tr>
	<td>-</td>
	<td>-</td>
	<td>TEMPLATE</td>
	<td>Template ID</td>
	<td>-</td>
	<td>8</td>
	<td>Integer (value = 25)</td>
	<td>-</td>
</tr>
<tr>
	<td>0 (Accel)</td>
	<td>0</td>
	<td>%Sens@Ref</td>
	<td>Sensitivity @ ref. condition</td>
	<td>CAL</td>
	<td>16</td>
	<td>ConRelRes (5E-7 to 172, ±0.015%)</td>
	<td>V/(m/s²)</td>
</tr>
<tr>
	<td>-</td>
	<td>-</td>
	<td>%TF_HP_S</td>
	<td>High pass cut-off freq. (F hp)</td>
	<td>CAL</td>
	<td>8</td>
	<td>ConRelRes (0.005 to 13k, ±3%)</td>
	<td>Hz</td>
</tr>
</table>

#### Further Information

* [LEE00] Lee, K. B.: IEEE 1451: A Standard in Support of Smart Transducer Networking http://www.nist.gov/manuscript-publication-search.cfm?pub_id=821920 Baltimore, MD, 01.01.2000
* [HBS] HBS: TEDS - memory module in transducer - Content and editing the memory module http://www.hbm.com/fileadmin/mediapool/hbmdoc/technical/a1813.pdf
* [NAT] National Instruments: An Overview of IEEE 1451.4 Transducer Electronic Data Sheets (TEDS) http://standards.ieee.org/develop/regauth/tut/teds.pdf
* IEEE 1451 -- A Universal Transducer Protocol Standard http://www.eesensors.com/DocsPDFs/ESP16_Atestp.pdf
* IEEE 1451 Family of Smart Transducer Interface Standards http://grouper.ieee.org/groups/1451/0/body%20frame_files/Family-of-1451_handout.htm

### Comparison

<table>
<tr>
	<th> </th>
	<th>SensorML</th>
	<th>TransducerML</th>
	<th>IEEE1451</th>
	<th>O&amp;M</th>
</tr>
<tr>
	<th>Sensor Data</th>
	<td>yes</td>
	<td>yes</td>
	<td>no</td>
	<td>yes</td>
</tr>
<tr>
	<th>Sensor Descr.</th>
	<td>yes</td>
	<td>yes</td>
	<td>yes</td>
	<td>no</td>
</tr>
<tr>
	<th>Mult. Sensor Interation</th>
	<td>no</td>
	<td>yes</td>
	<td>yes</td>
	<td>?</td>
</tr>
<tr>
	<th>Footprint</th>
	<td>medium</td>
	<td>medium</td>
	<td>small</td>
	<td>big</td>
</tr>
<tr>
	<th>Active</th>
	<td>yes</td>
	<td>?</td>
	<td>yes</td>
	<td>?</td>
</tr>
<tr>
	<th>Data Format</th>
	<td>XML</td>
	<td>XML</td>
	<td>propr.</td>
	<td>XML</td>
</tr>
</table>

### Conclusion

IEEE 1451 and SensorML are the two established standards. One of them should be used to describe sensors and their data.
