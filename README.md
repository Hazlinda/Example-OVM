# Example-OVM

Will verify the Switch RTL core using UVM in SystemVerilog. 

# Following are the steps we follow to verify the Switch RTL core. 

Building the Verification Environment. We will build the Environment in Multiple phases, so it will be easy to lean step by step. 
In this verification environment, It will not use agents and monitors to make this tutorial simple and easy. 

#Phase I : Top

We will develop the interfaces, and connect it to DUT in top module. 

#Phase II : Configuration

We will develop the Configuration class. 


    'ifndef GUARD_CONFIGURATION 
    'define GUARD_CONFIGURATION

    class Configuration extends uvm_object;
  
    virtual input_interface.IP input_intf;
    virtual mem_interface.MEM mem_intf;
    virtual output_interface.OP output_intf;
  
    bit [7:0] device_add[4];
  
    virtual function uvm_object create (string name="");
    Configuration t = new();
    
      t.device_add = this.device_add;
      t.input_intf = this.input_intf;
      t.mem_intf = this.output_intf;
    
    return t;
    endfunction : create
  
    endcase : Configuration
    'endif
    
# Phase III : Environment and Testcase

We will develop the Environment class and Simple testcase and simulate them. 



'ifndef GUARD_DRIVER 
'define GUARD_DRIVER

class Driver extends uvm_driver #(Packet);
  
  Configuration cfg;
  
  virtual input_interface.IP input_intf;
  virtual mem_interface.MEM mem_intf;
  
  uvm_analysis_port #(Packet) Drvr2Sb_port;
  
  'uvm_component_utils(Driver)
  
  function new (string name = "", uvm_component parent = null);
    super.new(name , parent);
  endfunction : new 
  
  virtual function void build();
    super.build();
    Drvr2SB_port = new ("Drvr2Sb", this);
  endfunction : build
