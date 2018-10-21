`timescale 1ns/1ns

module urna (
reset,clock,botao0,
    HEX0, sw);

    input reset;
    input clock;
    input botao0;
    //tri0 reset;
    //tri0 botao0;
    output HEX0;
    reg [6:0] HEX0;
    reg [6:0] reg_HEX0;
    reg [4:0] estado;
    reg [4:0] estado_atual;
	 input sw[2:0] number, numberCandidato1, numberCandidato2, numberCandidato3;
	 reg [6:0] candidato1, candidato2, candidato3;
	 reg block ;
	 reg rst;
	 
	 //parameter candidato1 =1, candidato2=2, candidato3;
	 reg [2:0] candidato1, candidato2, candidato3;
	 reg init;
	 
    parameter Espera=0,Apura=1,Computa=2,Exibe=3,Verifica=4;

    initial begin 
		estado_atual <= Espera;
    end  

	 //Da erro se Colocar o reset aqui!
    always @(posedge clock) begin
			
			case (estado_atual)

					Espera: begin
						HEX0 <= 7'b1001111;//1
						
					end
					
					Verifica: begin
					if(numbercandidato1 == 3'b001) // exibir o numero do candidato 1
					begin
					// código do display para exibir o numero no display
					//Chamar o modulo com o nome dele aqui(LCD)
					end	
					
					if(numbercandidato1 == 3'b010) // exibir o numero do candidato 2
					begin
					// código do display para exibir o numero no display
					//Chamar o modulo com o nome dele aqui(LCD)
					end	
					
					if(numbercandidato1 == 3'b011) // exibir o numero do candidato 3
					begin
					// código do display para exibir o numero no display
					//Chamar o modulo com o nome dele aqui(LCD)
					end	
					
					 Apura: begin
					 if(numbercandidato1 == 3'b001) // testa se o que está armazenado em numcandidato1 é o numero 1
					 begin // se for, computa o voto para candidato1
					 candidato1 = candidato1+1;
					 //Exibir mensagem de voto apurado
						HEX0 <= 7'b0000110; // 3	
					 end
					 
					 if(numbercandidato1 == 3'b010) // testa se o que está armazenado em numcandidato1 é o numero 2
					 begin 
					 candidato2 = candidato2+1;
					 //Exibir mensagem de voto apurado
						HEX0 <= 7'b0000110; // 3	
					 end
						
					if(numbercandidato1 == 3'b011) // testa se o que está armazenado em numcandidato1 é o numero 3
					 begin 
					 candidato3 = candidato3+1;
					 //Exibir mensagem de voto apurado
						HEX0 <= 7'b0000110; // 3	
					 end
				endcase
    end
	 

	always @( negedge botao0 or  negedge reset) begin
		
		if(!reset)begin 
			estado_atual = Espera;
		end
		
		else begin 
		
				case (estado_atual)

					Espera: begin

						if (botao0 == 0) 
							estado_atual <= Verifica;
						else
							estado_atual <= Espera;
					 end
					 
					 Verifica: begin
					 
						if(number == 3'b001) // caso o candidato 1 exista, deve ser exibido seu numero no outro always
						number = numberCandidato1;  //joga o número digitado pelo candidato para a variavel numberCandidato1 para
						// para ser verificado no outro always
							
						if(number == 3'b001 && botao0 == 1'b0)
						begin
						number = numberCandidato1;
						estado_atual<=apura;
						end
						
						if(number == 3'b010) // caso o candidato 2 exista, deve ser exibido seu numero e nome no outro always
						number = numberCandidato2; 
							
						if(number == 3'b010 && botao0 == 1'b0)
						begin
						number = numberCandidato2;
						estado_atual<=apura;
						end
						
						if(number == 3'b011) 
						number = numberCandidato3;  
							
						if(number == 3'b011 && botao0 == 1'b0)
						begin
						number = numberCandidato3;
						estado_atual<=apura;
						end
						
						else
							estado_atual <= Verifica;
					 end
					 
					 Apura: begin

						if (botao0 == 1'b0)
							estado_atual <= Espera;
						else
							estado_atual <= Apura;
						
					end
						
				endcase
		
		end

	end
	 
endmodule
