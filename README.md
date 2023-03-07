### METOT

package com.example.geridenkestirme;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {

    Button btn;
    EditText etXA;
    EditText etYA;
    EditText etXB;
    EditText etYB;
    EditText etXC;
    EditText etYC;
    EditText etKB;
    EditText etKC;
    EditText etKA;
    TextView twXK;
    TextView twYK;

    // Verilenler:
    // XA, YA, XB, YB, XC, YC; A, B, C bilinen sabit noktaların kartezyen koordinatları
    // KB, KC, KA; K bilinmeyen noktadan, A B C bilinen sabit noktalara doğrultu ölçüleri
    // İstenenler:
    // XK, YK; K bilinmeyen noktanın kartezyen koordinatları

    double XA;
    double YA;
    double XB;
    double YB;
    double XC;
    double YC;
    double KB;
    double KC;
    double KA;
    double XK;
    double YK;

    double ACI1;
    double ACI2;
    double ACI3;
    double ACI4;
    double ACI5;
    double ACI6;
    double ACI7;

    double FarkYAB;
    double FarkXAB;

    double FarkYCB;
    double FarkXCB;

    double tBA;
    double SBApayda;
    double SBA;

    double tBC;
    double SBCpayda;
    double SBC;

    double ACItoplam;
    double Sonuc1pay;
    double Sonuc1;
    double Birincieleman;
    double Ikincieleman;

    double Sonuc2;

    double tBK;
    double SBK;

    double pay;
    double payda;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btn= (Button) findViewById(R.id.button);
        etXA= (EditText) findViewById(R.id.editTextXA);
        etYA= (EditText) findViewById(R.id.editTextYA);
        etXB= (EditText) findViewById(R.id.editTextXB);
        etYB= (EditText) findViewById(R.id.editTextYB);
        etXC= (EditText) findViewById(R.id.editTextXC);
        etYC= (EditText) findViewById(R.id.editTextYC);
        etKB= (EditText) findViewById(R.id.editTextKB);
        etKC= (EditText) findViewById(R.id.editTextKC);
        etKA= (EditText) findViewById(R.id.editTextKA);
        twXK= (TextView) findViewById(R.id.textViewXK);
        twYK= (TextView) findViewById(R.id.textViewYK);

        btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                XA = Double.valueOf(etXA.getText().toString());
                YA = Double.valueOf(etYA.getText().toString());
                XB = Double.valueOf(etXB.getText().toString());
                YB = Double.valueOf(etYB.getText().toString());
                XC = Double.valueOf(etXC.getText().toString());
                YC = Double.valueOf(etYC.getText().toString());
                KB = Double.valueOf(etKB.getText().toString());
                KC = Double.valueOf(etKC.getText().toString());
                KA = Double.valueOf(etKA.getText().toString());

                //doğrultular GRAD'dan RADYAN'a dönüştürülüyor.

                KA = KA*2*3.14159265358979/400;
                KB = KB*2*3.14159265358979/400;
                KC = KC*2*3.14159265358979/400;

                //ALCI1 = α ; ACI2 = β

                ACI1 = KB - KA ;
                if (ACI1<0.0){
                    ACI1 = ACI1 + 2*3.14159265358979 ;
                }

                ACI2 = KC - KB ;
                if (ACI2<0.0){
                    ACI2 = ACI2 + 2*3.14159265358979 ;
                }

                FarkYAB = YA - YB ;
                FarkXAB = XA - XB ;

                FarkYCB = YC - YB ;
                FarkXCB = XC - XB ;

                //tBA, tBC RADYAN cinsinden doğrultular

                double div = (double) FarkYAB/FarkXAB;
                double theta = (double) (Math.atan(div));
                System.out.println(theta);

                tBA = theta;

                //GRAD cinsinde;
                // arctan(+/+)=ACI
                // arctan(-/-)=ACI ise ACI+200
                // arctan(+/-)=ACI ise ACI+200
                // arctan(-/+)=ACI ise ACI+400

                 if (FarkYAB<0.0 && FarkXAB<0.0){
                     tBA = tBA + 3.14159265358979;
                 }
                 else if (FarkYAB>0.00 && FarkXAB<0.0){
                     tBA = tBA + 3.14159265358979;
                 }
                 else if (FarkYAB<0.0 && FarkXAB>0.0){
                     tBA = tBA + 2*3.14159265358979;
                 }

                 //SBA, SBC metre cinsinden uzunluklar

                 SBApayda = Math.sin(tBA);
                 SBA = FarkYAB/SBApayda;

                double div1 = (double) FarkYCB/FarkXCB;
                double theta1 = (double) (Math.atan(div1));
                System.out.println(theta1);

                tBC = theta1;

                if (FarkYCB<0.0 && FarkXCB<0.0){
                    tBC = tBC + 3.14159265358979;
                }
                else if (FarkYCB>0.0 && FarkXCB<0.0){
                    tBC = tBC + 3.14159265358979;
                }
                else if (FarkYCB<0.0 && FarkXCB>0.0){
                    tBC = tBC + 2*3.14159265358979;
                }

                SBCpayda = Math.sin(tBC);
                SBC = FarkYCB/SBCpayda;

                //ACI3 = γ
                //γ = tBA - tBC

                ACI3 = tBA - tBC;
                if (ACI3<0.0){
                    ACI3 = ACI3 + 2*3.14159265358979;
                }
                else if (ACI3>2*3.14159265358979){
                    ACI3 = ACI3 - 2*3.14159265358979;
                }

                //Sonuc1 = ( φ + Ψ )/2
                //( φ + Ψ )/2 = ( 400 - ( α + β + γ))/2

                ACItoplam = ACI1 + ACI2 + ACI3;
                Sonuc1pay = 2*3.14159265358979 - ACItoplam;

                Sonuc1 = Sonuc1pay/2 ;

                //ACI6 = μ
                //tan( μ ) = (SBA * sin( β )) / (SBC * sin( α ))

                pay = SBA*Math.sin(ACI2);
                payda = SBC*Math.sin(ACI1);

                double div2 = (double) pay/payda;
                double theta2 = (double) (Math.atan(div2));
                System.out.println(theta2);

                ACI6 = theta2;

                //Sonuc2 = ( φ - Ψ )/2
                //tan( ( φ - Ψ )/2 ) = tan( ( φ + Ψ )/2 ) * cot(50 + μ)

                Birincieleman = Math.tan(Sonuc1);
                Ikincieleman = 1.0/(Math.tan((50*2*3.14159265358979/400)+ACI6));

                Sonuc2 = Math.atan(Birincieleman * Ikincieleman);

                //ACI4 = φ ; ACI5 = Ψ
                //ACI4 = ( φ + Ψ )/2 + ( φ - Ψ )/2
                //ACI5 = (φ + Ψ) - φ

                ACI4 = Sonuc1 + Sonuc2;
                ACI5 = 2.0*Sonuc1 - ACI4;

                //ACI7 = γ1
                //γ1 = 200 - ( φ + α)
                //γ2 = 200 - ( β + Ψ)
                //Kontrol: γ1 + γ2 = γ

                ACI7 = 3.14159265358979 - (ACI4 + ACI1);
                if (ACI7<0){
                    ACI7 = ACI7 + 2*3.14159265358979;
                }
                else if (ACI7>2*3.14159265358979){
                    ACI7 = ACI7 - 2*3.14159265358979;
                }


                tBK = tBA - ACI7;
                if (tBK<0.0){
                    tBK = tBK + 2*3.14159265358979;
                }
                else if (tBK>2*3.14159265358979){
                    tBK = tBK - 2*3.14159265358979;
                }

                SBK = SBA*((Math.sin(ACI4))/(Math.sin(ACI1)));

                XK = XB + SBK*Math.sin(tBK);
                YK = YB + SBK*Math.cos(tBK);

                twXK.setText(String.valueOf(String.format( "%.3f" , XK)));
                twYK.setText(String.valueOf(String.format( "%.3f" , YK)));


            }
        });

    }
}




TEST

Uygulama Prof.Dr. Vahap Engin GÜLAL hocamızın Ölçme Bilgisi Ders notlarından alınan örnek soru ile adım adım test edilmiştir.

![image](https://user-images.githubusercontent.com/114474881/223278136-d8d003cd-6de2-45c5-b7ea-ec7e2263447a.png)                             

![image](https://user-images.githubusercontent.com/114474881/223278181-142b1c9f-3214-4dee-a17b-726a0c5676ef.png)

![image](https://user-images.githubusercontent.com/114474881/223278218-cda7eee7-e97e-4c95-92d7-2c16b96de34d.png)

Uygulamaya “GRAD” cinsinden giriş yapılacaktır. Uygulama içerisinde “RADYAN” a çevirilip işlemler gerçekleştirilecektir.

 ![image](https://user-images.githubusercontent.com/114474881/223278290-557dae15-0e5e-40f6-8806-97cad02aa5b5.png)   ![image](https://user-images.githubusercontent.com/114474881/223278336-ea822a42-a58d-4782-9a8b-40226c88147f.png)   ![image](https://user-images.githubusercontent.com/114474881/223278382-0d78d407-c6e4-46d3-9311-0e79fb89bfb1.png)

Ders notunda sonuç aşağıdaki gibi belirlenmiştir:


 
