# buscaminas
juego
public class Buscamines {    
    static final int T=10;//tamany de la matriu visible    
    static final char TAPAT='X';
    static final char BANDERA= 'P';
    static final char RES= '_';
    static final char SEP='|';
    static final char MINA= 'Q';
    static final char bombes[][]=new char[T][T];
    static final char visible[][]=new char[T][T];
    
    public static void main(String[] args) {
        Scanner tec=new Scanner(System.in);
        
        int fil, col;
        boolean mines=true;
         
        for (int f = 0; f < visible.length; f++) {
            for (int c = 0; c < visible[f].length; c++) {
                visible[f][c]=RES;                             
            }           
        }
        
        System.out.println("Dis.me la fila");
        fil=tec.nextInt();
        System.out.println("Dis-me la columna");
        col=tec.nextInt();
        
        System.out.println("n mines "+minar(tec));
        System.out.println("char "+qma(fil, col));
        //System.out.println(minat(fil, col));
        destapar(fil, col);
        picar(fil, col);
        mostrarTauler();
        
       
    
    }
    
    static int minar(Scanner tec){       
        Random r=new Random();
        int contMines=0, mines, c, f;
  
//DEMANA QUANTITAT DE MINES I LES POSA ALEATORIAMENT        
        System.out.println("Numero de mines que vols");
        mines=tec.nextInt();
                      
        while(contMines < mines){
            f=r.nextInt(T);
            c=r.nextInt(T);

            if(bombes[f][c]!=MINA){
                contMines++;
            }
            bombes[f][c]=MINA;
        }
       return contMines;
    }
    
    static boolean minat(int f, int c){
       boolean minat = true;
       
            if(bombes[f][c] != MINA){
                minat=false;  //no ja mina
            }
            else{
                minat=true; //ja mina
            }       
       return minat;        
    }
    
    static boolean incorrecte(int f, int c){
        boolean incorrecte=true;
        
        if((f>=0 && f<=bombes.length-1) && 
                (c>=0 && c<=bombes.length-1)){
            incorrecte=false;
        }
        else{
            incorrecte=true;
        }                   
        return incorrecte;
    }
    
//per a contar les mines que hi han al voltant de la posicio pasada
    static char qma(int f, int c){
        int minAdj=0;
        System.out.println("f i c "+f+" "+c);
        if(bombes[f][c-1]== MINA){
          minAdj++;  
        }
        if(bombes[f][c+1]== MINA){
            minAdj++;
        }
        if(bombes[f-1][c]== MINA){
            minAdj++;
        }
        if(bombes[f+1][c]== MINA){
            minAdj++;
        }   
        if(bombes[f-1][c+1]== MINA){
            minAdj++;
        }
        if(bombes[f-1][c-1]== MINA){ 
            minAdj++;
        }                              
        if(bombes[f+1][c-1]== MINA){
            minAdj++;
        }
        if(bombes[f+1][c+1]== MINA){
            minAdj++;
        }
/*la variable minAdj es un enter i com que he de posar el numero de mines 
 que hi ha al voltant de la posicio en la matriu visible que es de char
 dons he de pasar el int a char.
 Puc fer-ho en su Switch, on li pase el contador i el retorne com a char, 
 o be fent una conversio*/        
        
        switch(minAdj){
            case 1: return '1';
            case 2: return '2';
            case 3: return '3';
            case 4: return '4';
            case 5: return '5';
            case 6: return '6'; 
            case 7: return '7';
            case 8: return '8'; 
            default: return ' ';
        }        
      //  return (char)minAdj;
    }
//per a comprobar que la posicio pasada està destapada o tapada    
    static boolean destapat(int f, int c){
        boolean destapat=true;
        
        if(visible[f][c]==TAPAT){
            destapat = false; //esta tapada
        }
        else{
            destapat = true;
        }
        return destapat;
    }
//per a saber la quantitat de posicions destapades    
    static int qdestapats(){
        int ndestapats=0, f=0, c=0;
        
        if(destapat(f, c)){
           ndestapats++; 
        }
        return ndestapats;
    }
    
    static void mostrarTauler(){

    //NUMEROS FILA DE DALT DEL TAULER 
        
        for (int f = 0; f < visible.length; f++) {            
            System.out.print("  "+(f));
        }
        System.out.println("");
        
        for (int f = 0; f < visible.length; f++) {               
            System.out.print((f)+""); //NUMEROS COLUMNA ESQUERRE DEL TAULER
            for (int c = 0; c < visible[f].length; c++) {                
                if (minat(f, c)){ /*MINES??*/
                    System.out.print(SEP+""+bombes[f][c]+" ");                    
                }
                else if(minat(f, c)!=true){                    
                    System.out.print(SEP+""+visible[f][c]+RES);                    
                }
            }
            System.out.print(SEP+""+f); //NUMEROS COLUMNA DRETA
            System.out.println("");
        }
    //NUMEROS FILA BAIX    
        for(int c=0; c<visible.length; c++){
            System.out.print("  "+(c)); //numeros columna baix
        }
        System.out.println(""); 
    }
    
    static void picar(int f, int c){        
        
        if(minat(f, c)){
            System.out.println("picat1"+minat(f, c));
            mostrarTauler();  
        }
        else{
            System.out.println("picat2");
            destapar(f, c);          
        }
    }
    
    static void destapar(int f, int c){
        
        if(incorrecte(f, c) || destapat(f, c)|| visible[f][c]==BANDERA){            
            //System.out.println(incorrecte(f, c)+" "+destapat(f, c));
            System.out.println("qma"+qma(f, c));
            if(qma(f, c)!='0'){
               System.out.println("qma"+qma(f, c));
               visible[f][c]=qma(f, c);
               
            } 
            else{
                visible[f][c]=RES;
                
                
                destapar(f,c-1);
                destapat(f, c+1);
                destapat(f-1, c);
                destapat(f+1, c);
                destapat(f-1, c+1);
                destapat(f-1, c-1);
                destapat(f+1, c-1);
                destapat(f+1, c+1);
            }            
        }        
    }
   
    
}
