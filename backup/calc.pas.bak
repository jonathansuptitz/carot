{ <description>

  Copyright (C) <2013> <Jonathan Eli Suptitz, Roberto Luiz Debarba> <jonny.suptitz@gmail.com, roberto.debarba@gmail.com>

  Este arquivo é parte do programa CAROT.

  CAROT é um software livre; você pode redistribuir e/ou modificá-los
  sob os termos da GNU Library General Public License como publicada pela Free
  Software Foundation; ou a versão 3 da Licença, ou (a sua escolha) qualquer
  versão posterior.

  Este código é distribuído na esperança de que seja útil, mas SEM
  QUALQUER GARANTIA; nem mesmo a garantia implícita de COMERCIABILIDADE ou
  ADEQUAÇÃO A UMA FINALIDADE PARTICULAR. Veja a licença GNU General Public
  License para maiores detalhes.

  Você deve ter recebido uma cópia da licença GNU Library General Public
  License juntamente com esta biblioteca; senão, escreva a Free Software
  Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA 02111-1307, USA.
}

unit calc;

{$MODE Delphi}

interface

uses
  LCLIntf, LCLType, SysUtils, Variants, Classes, Graphics, Controls, Forms,
  Dialogs, StdCtrls, ExtCtrls, DBCtrls;

type

    { TfrmCalc }

  TfrmCalc = class(TForm)
    bttSobre: TButton;
    Image1: TImage;
    Button4: TButton;
    Label2: TLabel;
    Label3: TLabel;
    Label5: TLabel;
    Label7: TLabel;
    Label8: TLabel;
    memo: TMemo;
    Panel1: TPanel;
    RadioButton1: TRadioButton;
    RadioButton2: TRadioButton;
    RadioButton3: TRadioButton;
    Label6: TLabel;
    Edit1: TEdit;
    Edit2: TEdit;
    StaticText3: TStaticText;
    procedure bttSobreClick(Sender: TObject);
    procedure Edit2KeyPress(Sender: TObject; var Key: char);
    procedure FormShow(Sender: TObject);
    procedure RadioButton1Change(Sender: TObject);
    procedure RadioButton2Change(Sender: TObject);
    procedure RadioButton3Change(Sender: TObject);
    function gerar:boolean;
    function erro:boolean;
    procedure Button4Click (Sender: TObject);
    procedure resultado;
    procedure Edit1KeyPress(Sender: TObject; var Key: Char);
    function OSVersion: string;
	private
    { Private declarations }
	public
    { Public declarations }
	end;

var
  frmCalc  : TfrmCalc;
  tempo  : string;
  delay2 : extended;
  movlw1 : integer;
  movlw2 : integer;
  movlw3 : integer;
  nop1   : integer;
  nop2   : integer;
  nop3   : integer;
  nop4   : integer;
  loop   : integer;
  aprox  : boolean;
  delay1 : extended;
  freq   : extended;
  arquivo, homedir : string;

implementation

uses  clipbrd, uSobre;

{$R *.lfm}

// Verifica O.S.
function TfrmCalc.OSVersion: string;
begin
  {$IFDEF LCLcarbon}
  OSVersion := 'Mac OS X 10';
  {$ELSE}
  {$IFDEF Linux}
  OSVersion := 'Linux Kernel';
  {$ELSE}
  {$IFDEF UNIX}
  OSVersion := 'Unix';
  {$ELSE}
  {$IFDEF WINDOWS}
  OSVersion:= 'Windows';
  {$ENDIF}
  {$ENDIF}
  {$ENDIF}
  {$ENDIF}
end;

procedure TfrmCalc.RadioButton1Change(Sender: TObject);
begin
    tempo := 'us';
end;

procedure TfrmCalc.RadioButton2Change(Sender: TObject);
begin
     tempo := 'ms';
end;

procedure TfrmCalc.RadioButton3Change(Sender: TObject);
begin
     tempo := 'seg';
end;

// trata campo frequencia assumir apenas numeros
procedure TfrmCalc.Edit2KeyPress(Sender: TObject; var Key: char);
begin
      if (not (Key in ['0'..'9'])) and (Key <> #8) then
         Key := #0;
end;

// trata campo frequencia assumir apenas numeros e define ponto como separador decimal
procedure TfrmCalc.Edit1KeyPress(Sender: TObject; var Key: Char);
begin
  if (not (Key in ['0'..'9', '.'])) and (key <> #8) then
    Key := #0;
end;

// inicialização da tela
procedure TfrmCalc.FormShow(Sender: TObject);
begin
   //centraliza form na tela
   FrmCalc.Top := (Screen.Height - FrmCalc.Height) div 2;
   FrmCalc.left := (Screen.Width - FrmCalc.Width) div 2;

    delay1 := 0;
    Edit1.text  := '0';
    tempo := 'us';

    // informacoes iniciais
     memo.Clear;
     memo.Lines.Add ('INFORMAÇÕES INICIAIS!');
     memo.Lines.Add (#13);
     memo.Lines.Add ('1 - Digite o a frequência do cristal.');
     memo.Lines.Add ('2 - Digite o valor a ser calculado.');
     memo.Lines.Add ('3 - Se necessário selecione o formato de tempo.');
     memo.Lines.Add ('3 - Clique em gerar.');
     memo.Lines.Add (#13);
     memo.Lines.Add ('Valores detempo muito grandes podem demorar mais que o normal para gerar o resultado.' + #13#10);
     memo.Lines.Add ('O resultado é automaticamente enviado para a área de transferência.');
end;

//PROCEDIMENTO BOTÃO GERAR CODIGO
procedure TfrmCalc.Button4Click(Sender: TObject);
begin
  gerar;
end;

// exibir erros
function TfrmCalc.erro: boolean;
begin
  result := false;
  if Length(Trim(Edit1.Text)) = 0 then begin
    showmessage('Digite o tempo!');
    result := true;
  end
  else if Length(Trim(Edit2.Text)) = 0 then begin
    showmessage('Digite a frequência!');
    result := true;
  end
  else if (delay1 < 8) and(tempo = 'us') then begin
     ShowMessage('Atenção: Digite um numero maior ou igual à 8 us!');
     result := true;
  end
  else if (delay1 > 99945.218) and (tempo = 'ms') then begin
    ShowMessage('Atenção: Digite um numero menor ou igual a 99.945 segundos!');
    result := true;
  end;
end;

//PROCEDIMENTO CALCULOS --------------------------------------------------------
function TfrmCalc.gerar : boolean;
var
  jaencontrou : boolean;
  ldiv1, ldiv2 : integer;
begin
  delay1 := StrToFloat(Edit1.text);

  if (tempo = 'us') and (delay1 > 1538) then begin
      tempo := 'ms';
      RadioButton2.checked := true;
      delay1 := delay1 / 1000;
  end
  else if (tempo = 'ms') and (delay1 < 1.538) then begin
      tempo := 'us';
      RadioButton1.checked := true;
      delay1 := delay1 * 1000;
  end
  else if (tempo = 'seg') then begin
      tempo := 'ms';
      RadioButton2.checked := true;
      delay1 := delay1 * 1000;
  end;

  Edit1.Text := FloatToStr(delay1);

  if not(erro) then begin
    freq := StrToFloat(Edit2.text) / 4;

    StaticText3.Visible := true; // exibe o 'aguarde' durante o calculo
    Application.ProcessMessages; // RESOLVE PROBLEMA EXIBIR 'AGUANDE' NO TRAVAMENTO

    delay2 := 0;
    aprox := false;
    jaencontrou := false;

    // INICIO TEMPO US < 1538
    if (delay1 <= 1538) and (delay1 >= 8) and (tempo = 'us') then begin
      loop := 1;
      for movlw1 := 1 to 255 do begin
        for nop1 := 0 to 3 do begin
          for nop2 := 0 to 3 do begin
            delay2 := ( (4 + (nop1 + 3 ) * movlw1 + nop2 + 1) * freq);
            if (delay1 = delay2) then begin
              aprox := false;
              resultado;
              result := true;
              exit;
            end;
          end;
        end;
      end;
    end
    // FIM TEMPO US < 1538

    else if (tempo = 'ms') then begin
      ldiv1 := 1;
      ldiv2 := 1;
      // INICIO TEMPO MS ENTRE 1.538 E 391.944
      if (delay1 > 1.538 ) and (delay1 <= 391.943) then begin
        loop := 2;
        while true do begin
          for nop3 := 0 to 3 do begin
            for nop2 := 0 to 3 do begin
              for nop1 := 0 to 3 do begin
                for movlw2 := ldiv2 to 255 do begin
                  for movlw1 := ldiv1 to 255 do begin
                    delay2 := ( ( ( 4 + ( 2 + (nop1 + 3) * movlw2 + nop2 + 2) * movlw1 + nop3 + 1 ) / 1000) * freq);
                    if ((delay1 = delay2) or (delay1 > (delay2 + 0.01))) then begin
                      aprox := false;
                      resultado;
                      exit;
                    end;
                    if (delay2 > delay1) then begin
                        if (movlw2 = 1) then begin
                           ldiv1 := movlw1 - 1;
                        end
                        else begin
                           ldiv2 := movlw2 - 1;
                        end;
                    end
                    else begin
                      if (movlw2 = 1) then begin
                         ldiv1 := movlw1 + 1;
                      end
                      else begin
                         ldiv2 := movlw2 + 1;
                      end;
                    end;
                  end;
                end;
     	      end;
     	    end;
          end;
        end;
      end
      // FIM TEMPO MS ENTRE 1.1538 E 391.944

      // INICIO TEMPO MS ENTRE 391.943 E 99945.219
      else if (delay1 > 391.943 ) and (delay1 <= 99945.218) then begin
        loop := 3;
        while true do begin
          for movlw1 := 1 to 255 do begin
            for movlw2 := 1 to 255 do begin
   	      for movlw3 := 1 to 255 do begin
     	        for nop1 := 0 to 3 do begin
                  for nop2 := 0 to 3 do begin
                   for nop3 := 0 to 3 do begin
                    for nop4 := 0 to 3 do begin
                       delay2 := ( ( (4 + (2 + (2 + (nop1 + 3) * movlw3 + nop2 + 2) * movlw2 + nop3 + 2) * movlw1 + nop4 + 1) / 1000) * freq);
                       if (delay1 = delay2) then begin
                        aprox := false;
                        resultado;
                        exit;
                       end
                       else if (delay2 > (delay1 - 0.01)) and (delay2 < (delay1 + 0.01)) then begin
                         if (jaencontrou) then begin
                           aprox := true;
                           resultado;
                           result := true;
                           exit;
                         end;
                         jaencontrou := true;
                      end;
                     end;
                   end;
                  end;
                end;
              end;
            end;
          end;
        end;
      end;
      // FIM TEMPO MS ENTRE 391.943 E 99945.219
    end;
    if (delay1 <> delay2) then begin
       ShowMessage('Valor sem resultado!');
    end;
  end;
  result := false;
end;

//PROCEDIMENTO EXIBIR RESULTADOS
procedure TfrmCalc.resultado;
var
  x : integer;
begin
  StaticText3.Visible := false;
  memo.Lines.Clear;

  if (aprox) then begin
      memo.Lines.Add('DELAY_'+ FloatToStr(delay2));
  end
  else begin
       memo.Lines.Add('DELAY_'+ FloatToStr(delay1));
  end;
  memo.Lines.Add('    MOVLW .'+ FloatToStr(movlw1));
  memo.Lines.Add('    MOVWF TEMPO0');
  memo.Lines.Add('LOOP1');

  if (loop > 1) then
  begin
     if(loop = 2) then
     begin
        memo.Lines.Add('    MOVLW .'+ FloatToStr(movlw2));
     end
     else begin
        memo.Lines.Add('    MOVLW .'+ FloatToStr(movlw3));
     end;
     memo.Lines.Add('    MOVWF TEMPO'+IntToStr(loop-1));
     memo.Lines.Add('LOOP'+IntToStr(loop));
  end;

  if (loop >= 1) then
  begin
    //nop1
    for x := 1 to nop1 do
    begin
        memo.Lines.Add('    NOP');
    end;
    memo.Lines.Add('    DECFSZ  TEMPO'+IntToStr(loop-1)+',F');
    memo.Lines.Add('    GOTO    LOOP'+ IntToStr(loop));
    //nop2
    for x := 1 to nop2 do
    begin
        memo.Lines.Add('    NOP');
    end;
  end;

  if (loop >= 2) then
  begin
      memo.Lines.Add('    DECFSZ  TEMPO'+IntToStr(loop-2)+',F');
      memo.Lines.Add('    GOTO    LOOP'+ IntToStr(loop-1));
      //nop3
      for x := 1 to nop3 do
      begin
        memo.Lines.Add('    NOP');
      end;
  end;

  if (loop = 3) then
  begin
      memo.Lines.Add('    DECFSZ  TEMPO'+IntToStr(loop-3)+',F');
      memo.Lines.Add('    GOTO    LOOP'+ IntToStr(loop-2));
      //nop4
      for x := 1 to nop4 do
      begin
        memo.Lines.Add('    NOP');
      end;
  end;
  memo.Lines.Add('RETURN');

  // informar aproximado ou nao
  if (aprox) then
    memo.Lines.Add(#13#10 +'; CÁLCULO APROXIMADO.')
  else
    memo.Lines.Add(#13#10 + '; CÁLCULO EXATO');

  // copia para area de transferencia o resultado
  Clipboard.AsText := memo.text;
end;

// sobre
procedure TfrmCalc.bttSobreClick(Sender: TObject);
begin
  frmSobre := TfrmSobre.Create(self);
  frmSobre.ShowModal;
end;
end.
