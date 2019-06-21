# verilog


Please see the JAN_1_19_ARRAY_MULTIPLIER.docx to view the block diagram of the array multiplier.



In verilogOutputSimpleSimulation.txt,

Here,a simulation for 4 bit adder is done and the results are written into a .txt file.The test vectors are taken from .txt file.
•	
			
		Use  $fread to open the file, $fwrite to write into .txt file.
•		

		After simulation,the results(sum and carry) must be written into the other.txt file.
•		

	Again,the elements of the .txt file must be now arranged in a matrix form,i.e,4 outputs
             Must be arranged as:

		Output1	Output2
		Output3	Output4
                                                    
.txt file


Implementation:
1.To write the contents(test vectors) from the .txt file,we follow:

The test vectors are taken into the testbench using  $readmemb command.

The test vectors are stored into the RAM memory defined by the number of test vectors as:

	Test vector bits	RAM name(register type)		Memory length
	   [3:0] 			 aRAM			[0:2]
	
	reg   [3:0]  aRAM [0:2];

2.We use a for loop to read the contents into input registers from the RAM memory.
******************************************************************************

	integer i;
	for( i = 0;i < 3;i = i + 1)
	begin
	a[i] = aRAM[i];
	$display(“ %b ”,a );  
	end
*************************************************************************************
3.Now,write the output vector into other .txt file using $fwrite operation.



COMPLETE Working:
1.Design a simple adder(4 bit adder) in verilog in Xilinx ISE.


miAdder.v

    module miAdder()
    input [3:0] a,b;
    output [3:0] sum;
    output carry;
    assign {c,sum} = a+b;
    endmodule
    
2.Now,attach a testbench tb_miAdder.v   


    module tb_four_bit_adder1;

	// Inputs
	reg [3:0] a;
	reg [3:0] b;

	// Outputs
	wire [3:0] sum;
	wire c;

	// Instantiate the Unit Under Test (UUT)
	four_bit_adder uut (
		.a(a), 
		.b(b), 
		.sum(sum), 
		.c(c)
	);


	reg [3:0] temp_aRAM [7:0];reg [3:0] temp_bRAM [7:0];
	reg [3:0] temp_sum [7:0];
	reg temp_c [7:0];
	integer i,fid;
	
	initial begin
		// Initialize Inputs
		a = 0;
		b = 0;

		// Wait 100 ns for global reset to finish
		#100;
        
		// Add stimulus here
		$readmemb("D:\\xilinx_projects\\krishnaSimulationTestBench\\testInputs\\input1.txt",temp_aRAM);
		
		$readmemb("D:\\xilinx_projects\\krishnaSimulationTestBench\\testInputs\\input2.txt",temp_bRAM);
		begin
		fid= $fopen("D:\\xilinx_projects\\krishnaSimulationTestBench\\testInputs\\output1.txt","w");
		end		
		
		for(i = 0 ;i < 8;i = i + 1)
			begin
			#40;
			a = temp_aRAM[i];
			b = temp_bRAM[i];
			#5;
			temp_sum[i] = sum;
			//$display("%b",{c, temp_sum[i]});
			$display("carry :%b ,sum : %b,total: %b",c,temp_sum[i],{c, temp_sum[i]});
			#10;
			//$fwrite(fid,"%b \t",sum1[i]);
			$fwrite(fid,"input1: %b,input2: %b,sum :%b ,carry : %b,total: %b \n",a,b,temp_sum[i],c,{c , temp_sum[i]});
			end		
		$fclose(fid);
	end
      
    endmodule
    
*********************************************************************************

      input1.txt
    0000	1001	0010	0100	0101	0110	1111

      input2.txt
    0001	1001	0010	0100	0101	0110	1111
*********************************************************************************** 
