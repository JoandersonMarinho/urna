module urna (clock, numero, confirma, reset, finaliza, hex0, hex1, hex2, hex3);

	input clock;
	input confirma;						//key0
	input reset;						//key1
	input finaliza;						//key2
	input [3:0] numero;					//switch
	
	output reg [6:0] hex0, hex1, hex2, hex3, hex4;				//display 7 seg
	
	reg [9:0] cand1 = 0, cand2 = 0, cand3=0, cand4=0;		//votos computados
	reg [3:0] estadoAtual = inicial;

	//Estados
	parameter inicial = 0, verifica = 1, exibeCandidato = 2, computaVoto = 3, exibeVencedor = 4;
	parameter numCand1 = 1, numCand2 = 2; numCand3=3, numCand=4;

	//Exibir Estado Atual
	always@(*) 
		
		begin
			case(estadoAtual)
				inicial: hex0 			= 	7'b1000000; // 0 
				verifica:
				begin
				
				
				
				exibeCandidato://Começo do estado que exibe o candidato
				begin
				if(numero==1) // caso o numero do candidato 1 seja selecionado nas chaves
				begin
				hex3 = 7'b1000110 //c
				hex2 = 7'b0001000;//a
				hex1 = 7'b0101011;//n
				hex0 = 7'b1001111;//1
				end
				
				else if(numero==2) // caso o numero do candidato 2 seja selecionado nas chaves
				begin
				
				hex3 = 7'b1000110;
				hex2 = 7'b0001000;
				hex1 = 7'b0101011;
				hex0=  7'b100100; //2
				end
				
				else if(numero ==3)  // caso o numero do candidato 3 seja selecionado nas chaves
				begin
				hex3 = 7'b1000110;
				hex2 = 7'b0001000;
				hex1 = 7'b0101011;
				hex0 = 7'b0110000; //3
				end
				
				else if(numero ==4) //  // caso o numero do candidato 4 seja selecionado nas chaves
				begin
				hex3 = 7'b1000110;
				hex2 = 7'b0001000;
				hex1 = 7'b0101011;
				hex0 = 7'b0011001;//4
				end
				
				end
				
				
				
				computaVoto: hex0 		<= 7'b0110000; // 3 
				exibeVencedor: hex0 	<= 7'b0011001; // 4
		endcase
		
	end	


	always@(posedge clock or negedge reset or negedge finaliza   ) 
	
		begin

			if(~finaliza)
				estadoAtual <= exibeVencedor;
			if(~reset)
				estadoAtual <= inicial;
			
			else begin
			
				case(estadoAtual)
					inicial : 
							if (numero != 4'b0000) estadoAtual <= verifica;
					
					verifica: 
							if(numero == numCand1)
								estadoAtual <= exibeCandidato; 
							else if(numero == numCand2)
								estadoAtual <= exibeCandidato;
							else estadoAtual <= inicial;
							  
					exibeCandidato: 
							if(!confirma)
								estadoAtual <= computaVoto;
							else estadoAtual <= inicial;
									
					computaVoto: estadoAtual <= inicial;
					
				endcase
			end


		end
		


endmodule 
