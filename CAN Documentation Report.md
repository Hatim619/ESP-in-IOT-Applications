

CAN Documentation

Report





**Understanding CAN in STM32f10x**

**Main Features**

**CAN in STM32f10x:**

Ø **2 CAN modules, CAN1 is “Master” meaning that it is**

**responsible for the controller CAN module configuration**

Ø **Filters using Message Identifier rather than Node Identifier,**

**Messages have priority not Nodes, Identifier is either Standard**

**(11 bits) or Extended (29 bits)**





**Understanding CAN in STM32f10x**

**Main Features**

**Transmission:**

Ø **Three Transmission Mailboxes**

Ø **Transmits by either configurable priority or First In First Out**

**(FIFO)**

**Reception:**

Ø **Two Receive FIFOs, each FIFO has three stages, can hold up to**

**three received messages, then FIFO is overrun**

Ø **28 Configurable Filter Banks between CAN1 and CAN2**





**Understanding CAN in STM32f10x**

**Main Features**

**Control, Status, Configuration Registers:**

Ø **Configures CAN Parameters (e.g., baud rate)**

Ø **Requests and Monitors Transmission**

Ø **Handles Receptions**

Ø **Manages Interrupt**

Ø **Get Diagnostic Information**

**CAN Operating Modes:**

Ø **Initialization Mode**

Ø **Normal Mode**

Ø **Sleep Mode**





**Understanding CAN in STM32f10x**

**CAN Operation Modes**

**Initialization Mode:**

Ø **On Hardware Reset, CAN Module is automatically on Sleep**

**Mode and Internal Pull-Up is active on CAN TX**

Ø **To Configure CAN you need to enter Initialization Mode where**

**the CAN Controller goes off CAN Bus**





**Understanding CAN in STM32f10x**

**CAN Operation Modes**

**Initialization Mode:**

Ø **To enter initialization mode, you need to Clear SLEEP bit and**

**set INRQ bit in CAN\_MCR, wait to confirm initialization mode**

**by hardware setting INAK bit and resetting SLAK bits in**

**CAN\_MSR**

Ø **CAN enters Normal Mode when both INAK and SLAK bits are**

**cleared by hardware, internal Pull-Up is disabled**





**Understanding CAN in STM32f10x**

**CAN Operation Modes**

**Initialization Mode:**

Ø **Before entering Normal Mode, CAN Synchronizes with CAN**

**bus, its waits for bus to be IDLE ( 11 consecutive recessive bits**

**monitored on CAN RX**

Ø **To initialize Filter Banks, FINIT bit in CAN\_FMR must be set,**

**when initialization is done it has to be reset again**





**Understanding CAN in STM32f10x**

**CAN Operation Modes**

**Sleep Mode:**

Ø **Default Controller Mode, can be entered by setting SLEEP bit**

**and waiting for hardware to confirm via SLAK bit**

Ø **If AWUM bit is set, CAN controller can automatically perform**

**wakeup sequence when is detects bus activity where it clears**

**SLEEP bit**





**Understanding CAN in STM32f10x**

**CAN Operation Modes**

**Normal Mode:**

Ø **CAN Controller enters CAN bus where is can transmit and**

**receive regularly**

Ø **Enters by clearing both INRQ and SLEEP bits and waiting for**

**hardware confirmation in INAK and SLAK bits**

Ø **Filter Scale and Mode Configuration must be configured before**

**entering Normal Mode**





**Understanding CAN in STM32f10x**

**CAN Testing Mode**

**Silent Mode:**

**Silent Mode**

Ø **One of three Modes used for CAN testing without the need of a**

**CAN transceiver provided by STM32f10x**

Ø **Entered by Setting SLIM bit in CAN\_BTR**

Ø **CAN only receives, transmission is routed back onto CAN RX,**

**CAN TX outputs only recessive bit 1**





**Understanding CAN in STM32f10x**

**CAN Testing Mode**

**Loop-Back Mode:**

**Loop-Back Mode**

Ø **Entered by Setting LBKM bit in CAN\_BTR**

Ø **CAN treats its own transmitted Message as received Message in**

**receive Mailbox**

Ø **Value of CAN RX pin is disregarded**





**Understanding CAN in STM32f10x**

**CAN Functional Description**

**Transmission Handling:**

Ø **Select 1 empty transmit Mailbox**

Ø **Set up Identifier**

Ø **Set up Data Length Code (DLC) (Number of Bytes you**

**will send, up to 8 bytes)**

Ø **Enter the Data**

Ø **Request Transmission (Set TXRQ bit)**





**Understanding CAN in STM32f10x**

**CAN Functional Description**

**Transmission Handling:**

Ø **Cannot write on Mailbox if not in Empty state**

Ø **TXRQ Set Mailbox Enter Pending state**

Ø **Waits Transmit Priority**

Ø **Message Enters Transmit state**

Ø **Transmission Complete RCQP, TXOK bits set**

Ø **Once transmission is complete, Mailbox goes returns to**

**Empty state**





**Understanding CAN in STM32f10x**

**CAN Functional Description**

**Transmission Handling:**

Ø **Can Abort Message Transmission by setting ABRQ bit**

Ø **When in Pending state, Automatic Retransmission of**

**message is default if transmission fails for any reason**

**such as lost bus arbitration**

Ø **Retransmission occurs when controller senses bus is**

**IDLE**

Ø **You can disable Automatic Retransmission of Message by**

**setting NART bit**





**Understanding CAN in STM32f10x**

**CAN Functional Description**

**Transmission Handling:**

Ø **Transmit Priority can be by Identifier (Lowest Identifier**

**value has highest Priority)**

Ø **If Identifier value is equal, Lower Mailbox is scheduled**

**first**

Ø **Transmit Priority can be configured as FIFO by setting**

**TXFP bit**

Ø **Interrupt TMEIE can be enabled to indicated**

**transmission done**





**Understanding CAN in STM32f10x**

**CAN Functional Description**

**Reception Handling:**

Ø **Three FIFOs, each FIFO takes up to three messages**

Ø **Valid Message if no error occurred (until last bit except**

**EOF field) and it passed through identifier filtering**

**successfully**





**Understanding CAN in STM32f10x**

**CAN Functional Description**

**Reception Handling:**

Ø **FIFO starts with an Empty state, then goes to Pending**

Ø **Hardware Signals event in FMP[1:0] in CAN\_RFR**

Ø **FMP[1:0] value indicates number of messages Pending**

Ø **When Message is available in FIFO output Mailbox:**

q **Read Mailbox**

q **Set RFOM bit in CAN\_RFR**

Ø **FIFO returns to Empty state**

Ø **If FIFO is in pending\_1 state and receives another**

**message, FIFO goes to pending\_2 state, then pending\_3**





**Understanding CAN in STM32f10x**

**CAN Functional Description**

**Reception Handling:**

Ø **If FIFO is Overrun, message will be either lost od**

**handled depending on configuration set**

Ø **3 Interrupts can be enabled:**

q **FMPIE when message is stored in FIFO**

q **FFIE when FIFO is full**

q **FOVIE when FIFO is Overrun**





**Understanding CAN in STM32f10x**

**CAN Identifier Filtering**

**Filter Configuration:**

Ø **Message Identifier is different form Node Address**

Ø **Software decides when messages to receive based on**

**identifier value**

Ø **28 Configurable Filter Banks between CAN1 and CAN2,**

**Number available to each is set by CAN2SB[5:0] in**

**CAN\_FMR**





**Understanding CAN in STM32f10x**

**CAN Identifier Filtering**

**Filter Configuration:**

Ø **Each Filter Bank consists of two 32-bit Registers**

**CAN\_FxR0 and CAN\_FxR1**

Ø **Each Filter Bank is scalable, either 16-bit or 32-bit mode**

Ø **Filter Can be either in Mask Mode or Identifier Mode**





**Understanding CAN in STM32f10x**

**CAN Identifier Filtering**

**Mask Mode:**

Ø **Identifier Registers are Mask Registers**

Ø **Mask Registers specifies which bits are “Must Watch”**

**or “don’t care” in identifier bits**

**Identifier Mode:**

Ø **All bits of the incoming identifier Must Match the bit**

**specified in filter register**





**Understanding CAN in STM32f10x**





**Understanding CAN in STM32f10x**

**CAN Identifier Filtering**

**Filter Configuration:**

Ø **Filter Banks are configured in CAN\_FMR**

Ø **You have first to clear FACT bit to deactivate filters**

**before configuring**

Ø **Filter Scale Configurated in CAN\_FS1R**

Ø **Identifier list or Mask mode for Corresponding Register**

**is configured in CAN\_FMR**





**Understanding CAN in STM32f10x**

**CAN Baud Rate Configuration**

**Bit Timing:**

Ø **The bit timing logic monitors the serial bus-line and performs**

**sampling and adjustment of the sample point by synchronizing**

**on the start-bit edge and resynchronizing on the following**

**edges.**

Ø **Configured using CAN\_BTR Register in initialization mode**

Ø **As a safeguard against programming errors, the configuration**

**of the Bit Timing Register**

Ø **(CAN\_BTR) is only possible while the device is in Standby**

**mode.**





**Understanding CAN in STM32f10x**

