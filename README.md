# Proyecto
///Objetivo: Mostrar de modo grafico como es una lista
///Autor:Moises Gonzalez Torres
///Fecha: 4 diciembre del 2017

///Librerias para el programa
#include <graphics.h>
#include <conio.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <stdio.h>
#include <string.h>

///Definicion de valores constantes
#define L 70 ///Es el largo que tendran nuestros nodos
#define R 6  ///Renglones de la malla
#define C 50  ///Columnas de nuestra malla

///Estructura de los botones

typedef struct {
int x1,y1;
int x2,y2;
char etiqueta[50];
int colorFondo;
}Boton;

///Estructura de los nodos
typedef struct NodoL
{
    int dato;
    int x1,y1; ///Datos de la posicion de los nodos de la lista
    struct NodoL *sig;
}*NL;

typedef struct Nodo
{
    int x,y; ///Datos de la posiciones de los nodos de la malla
    struct Nodo *Izq;
    struct Nodo *Abajo;
    struct Nodo *Arr;
    struct Nodo *Der;
}*NODO;

///Funciones para la insercion,eliminacion y crear nodos
void inicializar(NL *cab2,NODO cab);
int CreaNodo2(NL *cab2,int dato); //Crea los nodos de la lista
int InsercionIni(NL *cab2,NODO *cab,int dato);
int InsercionFin(NL *cab2,NODO *cab,int dato);
int InsercionRef(NL *cab2,NODO *cab,int refF);
void Recorrido(NL cab2);

int EliminaIni(NL *cab2);
int EliminaFin(NL *cab2);
int EliminaRef(NL *cab2,int ref);

///funcion que muestra la lista
void pintarNL(NL *cab2,NODO *cab);

///Funciones para los botones
void creaBotones(Boton btns[11]);
void dibujaBotones(Boton btns[11]);
void ControlaBotones(NL *cab2,NODO *cab,Boton btns[11]);


///Funciones para crear la malla
int CreaNodo(NODO *cab,int x, int y);
int Malla(NODO *cab);
void DibujaMalla(NODO cab);
void DibujaLista(NL cab2);

///funcion para solo capturar datos de la insercion inicio y final
int captura();
int capturaRef();

///Funciones para el scroll
void scrollDerecha(NL *cab2,NODO *cab);
void scrollIzquierda(NL *cab2,NODO *cab);

///Esta funcion dibuja la cabecera si es que apunta a algo o ya no apunta a nada
void dibujarCabecera(NL cab2,NODO cab);

///Nuestra funcion main llama a las demas funciones.
///funcion terminada
int main()
{
    int res;
    NODO cab;
    NL cab2;
    Boton btns[11];

    ///Llamada a la funcion que creea la malla
    res=Malla(&cab);

    if(res)
    {
        initwindow(1000,740,"Malla");
        srand(time(0));
        ///Si se creo la malla pasamos a donde nos muestra los botones que hacen lo demas
        ControlaBotones(&cab2,&cab,btns);
        closegraph();

    }
}


///Funcion para interactuar con el programa
void ControlaBotones(NL *cab2,NODO *cab,Boton btns[11])
{
  int opcion=-1,mx,my;
  int refI=0,refF=0,INI=0,INF=0,dato=0,ELIR;
  int res;
  NL aux2,Ant2;
  aux2=*cab2;
  Ant2=aux2;

    creaBotones(btns);
    dibujaBotones(btns);

    while(opcion!=11)
    {
     mx=mousex();
     my=mousey();



    if(ismouseclick(WM_LBUTTONDOWN))
        {


            ///verificar botones

            ///Inicializar Lista
            if(mx>btns[0].x1 && my>btns[0].y1 && my<btns[0].y2 && mx<btns[0].x2 )
                {
                    inicializar(&(*cab2),*cab);
                    opcion=1;
                }
                    ///Insertar Inicio
            if(mx>btns[1].x1 && my>btns[1].y1 && my<btns[1].y2 && mx<btns[1].x2 )
                {

                    clearmouseclick(WM_LBUTTONDOWN);

                    INI=captura();
                    res=InsercionIni(&(*cab2),&(*cab),INI);
                    opcion=2;
                }
                ///Insertar Final
            if(mx>btns[2].x1 && my>btns[2].y1 && my<btns[2].y2 && mx<btns[2].x2 )
                {
                    clearmouseclick(WM_LBUTTONDOWN);
                    INF=captura();
                    res=InsercionFin(&(*cab2),&(*cab),INF);
                    opcion=3;
                }

                ///Insertar por referencia
            if(mx>btns[3].x1 && my>btns[3].y1 && my<btns[3].y2 && mx<btns[3].x2 )
                {
                    clearmouseclick(WM_LBUTTONDOWN);

                    refI=capturaRef();
                    res=InsercionRef(cab2,cab,refI);
                    opcion=4;
                }

                ///Eliminar al Inicio
            if(mx>btns[4].x1 && my>btns[4].y1 && my<btns[4].y2 && mx<btns[4].x2 )
                {
                    clearmouseclick(WM_LBUTTONDOWN);
                     res=EliminaIni(&(*cab2));
                    opcion=5;
                }

                ///Eliminar al Final
            if(mx>btns[5].x1 && my>btns[5].y1 && my<btns[5].y2 && mx<btns[5].x2 )
                {
                    clearmouseclick(WM_LBUTTONDOWN);
                    res=EliminaFin(&(*cab2));
                    opcion=6;
                }

                ///Eliminar por referencia
            if(mx>btns[6].x1 && my>btns[6].y1 && my<btns[6].y2 && mx<btns[6].x2 )
                {
                    clearmouseclick(WM_LBUTTONDOWN);
                    ELIR=captura();
                    res=EliminaRef(&(*cab2),ELIR);
                    ELIR=0;
                    opcion=7;
                }

                    ///Movimiento izquierda
            if(mx>btns[8].x1 && my>btns[8].y1 && my<btns[8].y2 && mx<btns[8].x2)
                {
                    scrollIzquierda(cab2,cab);
                    opcion=8;
                }
                    ///Movimiento derecha
            if(mx>btns[9].x1 && my>btns[9].y1 && my<btns[9].y2 && mx<btns[9].x2)
                {

                    scrollDerecha(cab2,cab);
                    opcion=9;
                }

                    ///Recorrido
            if(mx>btns[7].x1 && my>btns[7].y1 && my<btns[7].y2 && mx<btns[7].x2)
                {
                    Recorrido(*cab2);
                    opcion=10;
                }

                    ///Salir
            if(mx>btns[10].x1 && my>btns[10].y1 && my<btns[10].y2 && mx<btns[10].x2 )
                {
                    opcion=11;
                }

                if(opcion!=11)
                {
                    ///Se les da un valor a la opcion despues de cada boton para redibujar la lista y la cabecera
                    if(opcion!=1)
                    {
                            pintarNL(cab2,cab);
                    }
                    creaBotones(btns);
                    dibujaBotones(btns);
                  opcion=0;
                }
                clearmouseclick(WM_LBUTTONDOWN);
            }
    }
}

///Funcion de crea nodo para la lista
///funcion terminada
int CreaNodo2(NL *cab2,int dato)
{
    int res=0;
    *cab2=(NL)malloc(sizeof(struct NodoL));
    if(*cab2)
    {

        (*cab2)->dato=dato;
        (*cab2)->sig=NULL;
        res=1;
    }
    return (res);
}

///Funcion que crea los nodos para nuestra malla
///funcion terminada
int CreaNodo(NODO *cab,int x, int y)
{
    int res=0;
    *cab=(NODO)malloc(sizeof(struct Nodo));
    if(*cab)
    {
        (*cab)->x=x;
        (*cab)->y=y;
        (*cab)->Izq=NULL;
        (*cab)->Arr=NULL;
        (*cab)->Abajo=NULL;
        (*cab)->Der=NULL;

        res=1;
    }
    return (res);
}


///Esta es la funcion que crea nuestra malla
///funcion terminada
int Malla(NODO *Cab)
{
int x, y=50, i,j,res=0,dato=0;
NODO atras=NULL, arriba=NULL, aux=NULL,Nuevo;
for(i=0; i<R; i++)
    {
        x=50;
        atras=NULL;
        arriba=aux;
        for(j=0; j<C; j++)
        {
            res=CreaNodo(&Nuevo,x+(j*L),y+(i*L));
                if(res)
                    {
                        if(j==0)
                            {
                                aux=Nuevo;
                                    if(i==0)
                                        *Cab=Nuevo;
                            }
                        if(atras!=NULL)
                            {
                               Nuevo->Izq=atras;
                               atras->Der=Nuevo;
                            }

                        if(arriba!=NULL)
                            {
                                Nuevo->Arr=arriba;
                                arriba->Abajo=Nuevo;
                                arriba=arriba->Der;
                            }
                        atras=Nuevo;
    }
  }
            }
    return (res);
}

///Funcion que dibuja malla
///Solo se uso y se puede usar para ver las posiciones de la misma y los de la lista
void DibujaMalla(NODO Cab)
{
int i, j;
NODO Aux, Down;
Down=Cab;
Aux=Down;

for (i=0;i<R;++i)
{
  Aux=Down;
  for (j=0;j<C;++j)
  {
   setcolor(5);
   rectangle((Aux->x),Aux->y,(Aux->x+L),Aux->y+L);

  Aux=Aux->Der;
  }
  Down=Down->Abajo;
  }
}

///funcion para mover el scroll a la derecha
///funcion ya terminada
void scrollDerecha(NL *cab2,NODO *cab)
{
    int j;
    NL auxL,auxL2;
    NODO auxM,derecha,abajo;
    auxL=*cab2;
    auxM=*cab;
    derecha=abajo=*cab;

    cleardevice();

    while(abajo)
    {
        j=1;
        while(derecha)
        {
              (derecha->x)=(derecha->x)+50;
              j++;
              derecha=derecha->Der;
        }
        abajo=abajo->Abajo;
        derecha=abajo;
    }

}


///funcion para mover el scroll a la derecha
///funcion ya terminada
void scrollIzquierda(NL *cab2,NODO *cab)
{

    NL auxL,auxL2;
    NODO auxM,derecha,abajo;
    auxL=*cab2;
    auxM=*cab;
    derecha=abajo=*cab;

    cleardevice();

    while(abajo)
    {
        while(derecha)
        {
              (derecha->x)=(derecha->x)-50;
              derecha=derecha->Der;
        }
        abajo=abajo->Abajo;
        derecha=abajo;
    }

}


///Esta funcion crea los botones que moveran todo el programa
///funcion terminada
void creaBotones(Boton btns[11])
{
    int xi=810,yi=300,i;

    for(i=0;i<8;i++)
    {
    btns[i].x1=xi;
    btns[i].y1=yi;
    btns[i].x2=980;
    btns[i].y2=yi+30;
    btns[i].colorFondo=500;
    if(i==0)
    {
    strcpy(btns[i].etiqueta,"Inicializar");
    }
    if(i==1)
    {
    strcpy(btns[i].etiqueta,"Insertar al inicio");
    }

    if(i==2)
    {
    strcpy(btns[i].etiqueta,"Inserta al final");
    }
    if(i==3)
    {
    strcpy(btns[i].etiqueta,"Inserta por referencia");
    }
    if(i==4)
    {
    strcpy(btns[i].etiqueta,"Elimina al inicio");
    }
    if(i==5)
    {
    strcpy(btns[i].etiqueta,"Eliminar al final");
    }
    if(i==6)
    {
    strcpy(btns[i].etiqueta,"Eliminar por referencia");
    }
    if(i==7)
    {
    strcpy(btns[i].etiqueta,"Recorrido");
    }
    yi+=50;
    }


    btns[8].x1=300;
    btns[8].y1=650;
    btns[8].x2=400;
    btns[8].y2=680;
    btns[8].colorFondo=500;
    strcpy(btns[8].etiqueta,"<");


    btns[9].x1=450;
    btns[9].y1=650;
    btns[9].x2=550;
    btns[9].y2=680;
    btns[9].colorFondo=500;
    strcpy(btns[9].etiqueta,">");


    btns[10].x1=xi;
    btns[10].y1=yi;
    btns[10].x2=980;
    btns[10].y2=yi+30;
    btns[10].colorFondo=500;
    strcpy(btns[10].etiqueta,"Salir");
}

///Funcion que dibuja los botones
///Funcion terminada
void dibujaBotones(Boton btns[11])
{
    int i;
    setcolor(WHITE);
    for(i=0;i<11;i++)
    {
    rectangle(btns[i].x1,btns[i].y1,btns[i].x2,btns[i].y2);

    setfillstyle(SOLID_FILL,btns[i].colorFondo);

    floodfill(btns[i].x1+2,btns[i].y1+2,WHITE);

    outtextxy(btns[i].x1+18,btns[i].y1+10,btns[i].etiqueta);
    }
}


///Funcion para inicializar lista
///Funcion terminada
void inicializar(NL *cab2,NODO cab)
{
    *cab2=NULL;

        setcolor(WHITE);
        rectangle(cab->x,cab->y,cab->x+L,cab->y+L);

        outtextxy(cab->x+5,cab->y+5,"Cabecera");
}

///funcion de dibujar cabecera
void dibujarCabecera(NL cab2,NODO cab)
{
    NODO aux;
    aux=cab;
    if(cab2)
    {
        /// Despues de inicializarla se le da una posicion donde se visualizaria de donde estaria nuestra cabecera
        ///Dibuja o borra si es que apunta o si ya no lo hace la borra
        setcolor(WHITE);
        rectangle(aux->x,aux->y,aux->x+L,aux->y+L);

        outtextxy(aux->x+5,aux->y+5,"Cabecera");
    }
    else{
        /// si no apunta a nada o ya se eliminaron todos los datos la cabecera se borra
        cleardevice();
    }
}

///Funcion para capturar datos de insercion inicio o final
///Funcion terminada
int captura()
{
int res,cont=0,x=200,y=550;
char aux[5],aux2[5];
aux[cont]=0;


    while(!GetAsyncKeyState(13)){  ///El ciclo continua mientras no se de enter
        setcolor(RED);
        outtextxy(100,y,"Escribe dato");
        aux[cont]=getch();
        res=atoi(aux);
        cont++;
        sprintf(aux2,"%d",res);
        outtextxy(x,y,aux2);
    }

        fflush(stdin);  ///Limpia el buffer de entrada estandar del teclado
     ///Limpiamos el buffer despues del ciclo para que cuando ingrese a la otra funcion de captura referencia o visecersa no detecte el ultimo enter
        res=atoi(aux);  ///Convierte de char a int
   return (res);
}

///Esta funcion se llama para capturar el dato que funcionara como referencia hacia la insercion
int capturaRef()
{
int res,cont=0,x=250,y=620,y2=640;
char aux[5],aux2[5];
aux[cont]=0;

    while(!GetAsyncKeyState(13)){  ///El ciclo continua mientras no se de enter
        setcolor(RED);
        outtextxy(100,y,"Escribe referencia");
        aux[cont]=getch();
        res=atoi(aux);
        cont++;
        sprintf(aux2,"%d",res);
        outtextxy(x,y,aux2);
    }
     fflush(stdin);  ///Limpia el buffer de entrada estandar del teclado
     ///Limpiamos el buffer despues del ciclo para que cuando ingrese a la otra funcion de captura referencia o visecersa no detecte el ultimo enter

        res=atoi(aux);  ///Convierte de char a int

   return (res);
}

///Funcion que inserta el nodo al inicio
///Funcion terminada
int InsercionIni(NL *cab2,NODO *cab,int dato)
{
    int res,i=0;
    char cad[5];
    NL nuevo,auxL;
    NODO aux,ant;
    aux=*cab;
    ant=aux;

    ant=ant->Abajo;
    ant=ant->Abajo;
    ant=ant->Abajo;
    ant=ant->Abajo;

    res=CreaNodo2(&nuevo,dato);

    if(res){


        setcolor(BLUE);
        rectangle(ant->x,ant->y,ant->x+L,ant->y+L);
        setcolor(GREEN);
        outtextxy(ant->x+5,ant->y+100,"Nuevo dato de entrada");  ///Estas lineas dibujan en una parte de la malla el nuevo dato que va a entrar al principio

        if(nuevo->dato != -1)
           {
               sprintf(cad,"%i",nuevo->dato);
           }
            else{
            cad[0]='%0';
            cad[1]='%0';
           }
           outtextxy((ant->x)+30,(ant->y)+30,cad);

            setcolor(WHITE);

            line(ant->x,ant->y-20,ant->Arr->x,ant->Arr->y); ///Muestra la linea que apunta a ser el primero
            line(ant->x+50,ant->y,ant->Arr->x+50,ant->Arr->y+50);
            line(ant->Arr->x+50,ant->Arr->y+50,ant->x,ant->y-20);
            line(ant->Arr->x,ant->Arr->y,ant->Arr->x-10,ant->Arr->y+10); ///Forman la punta de la flecha que dice que va a entrar primero
            line(ant->Arr->x,ant->Arr->y,ant->Arr->x+10,ant->Arr->y+10);

            nuevo->sig=*cab2;
            *cab2=nuevo;
            res=1;
           }
              delay(3000);
    return (res);
}


///Funcion que inserta el nodo al final
///Funcion terminada
int InsercionFin(NL *cab2,NODO *cab,int dato)
{
    int res,i=0;
    NL nuevo,auxL;
    auxL=*cab2;
    NODO aux,ant;
    aux=ant=*cab;
    char cad[5];


    ant=ant->Abajo;
    ant=ant->Abajo;
    ant=ant->Abajo;
    ant=ant->Abajo;

    res=CreaNodo2(&nuevo,dato);

    if(res)
    {
        if(!*cab2){
        *cab2=nuevo;

        setcolor(BLUE);
        rectangle(ant->x,ant->y,ant->x+L,ant->y+L);
        setcolor(GREEN);
        outtextxy(ant->x+5,ant->y+100,"Nuevo dato de entrada");  ///Estas lineas dibujan en una parte de la malla el nuevo dato que va a entrar al principio

        if(nuevo->dato != -1)
           {
               sprintf(cad,"%i",nuevo->dato);
           }
            else{
            cad[0]='%0';
            cad[1]='%0';
           }
           outtextxy((ant->x)+30,(ant->y)+30,cad);

            setcolor(WHITE);

            line(ant->x,ant->y-20,ant->Arr->x,ant->Arr->y); ///Muestra la linea que apunta a ser el primero
            line(ant->x+50,ant->y,ant->Arr->x+50,ant->Arr->y+50);
            line(ant->Arr->x+50,ant->Arr->y+50,ant->x,ant->y-20);
            line(ant->Arr->x,ant->Arr->y,ant->Arr->x-10,ant->Arr->y+10); ///Forman la punta de la flecha que dice que va a entrar primero
            line(ant->Arr->x,ant->Arr->y,ant->Arr->x+10,ant->Arr->y+10);

        }
        else{
        auxL=*cab2;
        while(auxL->sig)
        {
            ant=ant->Der;
            auxL=auxL->sig;
        }

        setcolor(BLUE);
        rectangle(ant->x,ant->y,ant->x+L,ant->y+L);
        setcolor(GREEN);
        outtextxy(ant->x+5,ant->y+100,"Nuevo dato de entrada");  ///Estas lineas dibujan en una parte de la malla el nuevo dato que va a entrar al principio

        if(nuevo->dato != -1)
           {
               sprintf(cad,"%i",nuevo->dato);
           }
            else{
            cad[0]='%0';
            cad[1]='%0';
           }
           outtextxy((ant->x)+30,(ant->y)+30,cad);

            setcolor(WHITE);

            line(ant->x+L,ant->y-20,ant->Arr->x+L,ant->Arr->y); ///Muestra la linea que apunta a ser el primero
            line(ant->Arr->x+20,ant->Arr->y+50,ant->x+L,ant->y-20);
            line(ant->x+20,ant->y,ant->Arr->x+20,ant->Arr->y+50);
            line(ant->Arr->x+L,ant->Arr->y,ant->Arr->x-10+L,ant->Arr->y+10); ///Forman la punta de la flecha que dice que va a entrar primero
            line(ant->Arr->x+L,ant->Arr->y,ant->Arr->x+10+L,ant->Arr->y+10);

        auxL->sig=nuevo;
        }
        res=1;
    }

    delay(3000);

    return (res);
}

///Funcion terminada
int InsercionRef(NL *cab2,NODO *cab,int ref)
{
    int res,res_dato;
    char cad[5];
    NODO auxM;
    NL auxL,antL,nuevo;
    auxL=antL=*cab2;

    if(!*cab2)
    {
     res=0;
    }
    else{
        if(ref==(*cab2)->dato)
        {
            delay(800);
            res_dato=captura();
            res=CreaNodo2(&nuevo,res_dato);

            auxM=*cab;

            auxM=auxM->Abajo;
            auxM=auxM->Abajo;
            auxM=auxM->Abajo;
            auxM=auxM->Abajo;

            setcolor(BLUE);
            rectangle(auxM->x,auxM->y,auxM->x+L,auxM->y+L);
            setcolor(GREEN);
            outtextxy(auxM->x+5,auxM->y+100,"Nuevo dato de entrada");  ///Estas lineas dibujan en una parte de la malla el nuevo dato que va a entrar al principio

            if(nuevo->dato != -1)
                {
                    sprintf(cad,"%i",nuevo->dato);
                }
                else{
                    cad[0]='%0';
                    cad[1]='%0';
                }
            outtextxy((auxM->x)+30,(auxM->y)+30,cad);

            setcolor(WHITE);

            line(auxM->x,auxM->y-20,auxM->Arr->x,auxM->Arr->y); ///Muestra la linea que apunta a ser el primero
            line(auxM->x+50,auxM->y,auxM->Arr->x+50,auxM->Arr->y+50);
            line(auxM->Arr->x+50,auxM->Arr->y+50,auxM->x,auxM->y-20);
            line(auxM->Arr->x,auxM->Arr->y,auxM->Arr->x-10,auxM->Arr->y+10); ///Forman la punta de la flecha que dice que va a entrar primero
            line(auxM->Arr->x,auxM->Arr->y,auxM->Arr->x+10,auxM->Arr->y+10);

            delay(3000);
            if(res)
            {
                nuevo->sig=*cab2;
                *cab2=nuevo;
            }
        }
        else{
        res=InsercionRef(&(*cab2)->sig,&(*cab)->Der,ref);
        }


        return (res);
    }
}


///Funcion terminada
int EliminaIni(NL *cab2)
{
    int res;
    NL aux,aux2;
    char cad[5];

    aux2=*cab2;

   while(aux2->sig != NULL)
   {
    aux2=aux2->sig;
   }


    if(*cab2)
    {
        aux=*cab2;

               setcolor(WHITE);
               line(aux->x1-20,aux->y1-20,aux->x1+L+20,aux->y1+L+20);
               outtextxy(aux->x1-50,aux->y1-20,"Eliminar primer dato");

                delay(3000);

               setcolor(BLACK);
               line(aux->x1-20,aux->y1-20,aux->x1+L+20,aux->y1+L+20);
               outtextxy(aux->x1-50,aux->y1-20,"Eliminar primer dato");

               setcolor(BLACK);
            rectangle(aux2->x1,aux2->y1,aux2->x1+L,aux2->y1+L);

           if(aux2->dato != -1)
           {
               sprintf(cad,"%i",aux2->dato);
           }
           else{
            cad[0]='%0';
            cad[1]='%0';
           }
           setcolor(BLACK);
           outtextxy((aux2->x1)+7,(aux2->y1)+5,cad);
           setcolor(BLACK);
           line(aux2->x1+30,aux2->y1,aux2->x1+30,aux2->y1+L);
           setcolor(BLACK);
           line(aux2->x1+30,aux2->y1+L,aux2->x1+L,aux2->y1);

        *cab2=(*cab2)->sig;
        aux2=*cab2;


        free(aux);
        res=1;
    }

    return (res);
}

///Funcion que nos elimina el ultimo nodo
///Funcion terminada
int EliminaFin(NL *cab2)
{
    int res=0;
    NL aux,ant;
    char cad[5];

    if(*cab2)
    {
        aux=*cab2;
        while(aux->sig)
        {
            ant=aux;
            aux=aux->sig;
        }
        if(aux==*cab2)
        {

               setcolor(WHITE);
               line(aux->x1-20,aux->y1-20,aux->x1+L+20,aux->y1+L+20);
               outtextxy(aux->x1-50,aux->y1-20,"Eliminar ultimo dato");

                delay(3000);

               setcolor(BLACK);
               line(aux->x1-20,aux->y1-20,aux->x1+L+20,aux->y1+L+20);
               outtextxy(aux->x1-50,aux->y1-20,"Eliminar ultimo dato");


            setcolor(BLACK);
            rectangle(aux->x1,aux->y1,aux->x1+L,aux->y1+L);


           if(aux->dato != -1)
           {
               sprintf(cad,"%i",aux->dato);
           }
           else{
            cad[0]='%0';
            cad[1]='%0';
           }
           setcolor(BLACK);
           outtextxy((aux->x1)+7,(aux->y1)+5,cad);
           setcolor(BLACK);
           line(aux->x1+30,aux->y1,aux->x1+30,aux->y1+L);
           setcolor(BLACK);
           line(aux->x1+30,aux->y1+L,aux->x1+L,aux->y1);


            *cab2=NULL;

        }
        else{
            setcolor(WHITE);
               line(aux->x1-20,aux->y1-20,aux->x1+L+20,aux->y1+L+20);
               outtextxy(aux->x1-50,aux->y1-20,"Eliminar ultimo dato");

                delay(3000);

               setcolor(BLACK);
               line(aux->x1-20,aux->y1-20,aux->x1+L+20,aux->y1+L+20);
               outtextxy(aux->x1-50,aux->y1-20,"Eliminar ultimo dato");


            setcolor(BLACK);
            rectangle(aux->x1,aux->y1,aux->x1+L,aux->y1+L);

           if(aux->dato != -1)
           {
               sprintf(cad,"%i",aux->dato);
           }
           else{
            cad[0]='%0';
            cad[1]='%0';
           }
           setcolor(BLACK);
           outtextxy((aux->x1)+7,(aux->y1)+5,cad);
           setcolor(BLACK);
           line(aux->x1+30,aux->y1,aux->x1+30,aux->y1+L);
           setcolor(BLACK);
           line(aux->x1+30,aux->y1+L,aux->x1+L,aux->y1);

        ant->sig=NULL;
        free(aux);
        res=1;
        }

    }

    return (res);
}


///Funcion no terminada
int EliminaRef(NL *cab2,int ref)
{
    int res=0;
    NL ant,aux;

    if(*cab2)
    {
        aux=*cab2;
        while(aux&&aux->dato != ref)
        {
            ant=aux;
            aux=aux->sig;
        }
        if(aux==*cab2)
        {
               setcolor(WHITE);
               line(aux->x1-20,aux->y1-20,aux->x1+L+20,aux->y1+L+20);
               outtextxy(aux->x1-50,aux->y1-20,"Eliminar dato referencia");
               delay(3000);

               *cab2=NULL;
        }
        else{
               setcolor(WHITE);
               line(aux->x1-20,aux->y1-20,aux->x1+L+20,aux->y1+L+20);
               outtextxy(aux->x1-50,aux->y1-20,"Eliminar dato referencia");
               delay(3000);

               if(aux)
               {
                   if(aux==*cab2)
                   {
                       *cab2=(*cab2)->sig;
                   }
                   else{
                        ant->sig=aux->sig;
                   }
                   res=1;
               }
               free(aux);
        }
    }
    cleardevice();
    return (res);
}

///Esta funcion hace simulacion del recorrido
void Recorrido(NL cab2)
{
    NL aux,ant;
    ant=aux=cab2;

    if(aux)
    {

        while(aux)
        {
            setcolor(GREEN);
            outtextxy(aux->x1+5,aux->y1+90,"Cabecera");
            setcolor(GREEN);
            rectangle(aux->x1,aux->y1,aux->x1+L,aux->y1+L);

            delay(3000);
            setcolor(WHITE);
            rectangle(aux->x1,aux->y1,aux->x1+L,aux->y1+L);

            setcolor(BLACK);
            outtextxy(aux->x1+5,aux->y1+90,"Cabecera");
            ant=aux;
            aux=aux->sig;
        }
    }

    cleardevice();
}

///Esta funcion es la que nos pinta la lista despues de cada insercion o eliminacion de la misma
///Funcion terminada
void pintarNL(NL *cab2,NODO *cab)
{
    char cad[5];
    NODO aux,aux2;
    NL auxL,auxL2;
    auxL=*cab2;
    auxL2=auxL;
    aux=*cab;
    aux=aux->Abajo;
    aux=aux->Abajo;
    aux2=aux;

    cleardevice();


    if(*cab2) ///Este if se incluye por el hecho de que cuando el usuario llega a eliminar todo los datos la cabecera ya no apunta a nada y por ende ya no se puede dibujar nada
    {

        dibujarCabecera(*cab2,*cab);

        while(auxL && aux)
        {
               auxL->x1=aux->x;
               auxL->y1=aux->y;

               setcolor(WHITE);
               rectangle(auxL->x1,auxL->y1,auxL->x1+L,auxL->y1+L); ///Dibuja nuestro nodo

           if(auxL->dato != -1)
           {
               sprintf(cad,"%i",auxL->dato);
           }
           else{
            cad[0]='%0';
            cad[1]='%0';
           }
            outtextxy((auxL->x1)+7,(auxL->y1)+5,cad);
            setcolor(WHITE);
            line(auxL->x1+30,auxL->y1,auxL->x1+30,auxL->y1+L); ///Dibuja la particion donde divide nuestro dato y si apunta a otro nodo o no

           if(auxL->sig != NULL)
           {

                ///El primer black es para "borrar la lineas y sobre escribir
               setcolor(WHITE);
               line(auxL->x1+30,auxL->y1+(L/2),auxL->x1+L,(auxL->y1)+(L/2)); ///Crea Linea si esta ligado

               setcolor(WHITE);
               line(auxL->x1+50,auxL->y1-20+L,auxL->x1+L,(auxL->y1-35)+L); ///Crea pedazo flecha

               setcolor(WHITE);
               line(auxL->x1+50,auxL->y1-50+L,auxL->x1+L,(auxL->y1-35)+L); ///Crea el otro pedazo de la flecha


           }
           if(auxL->sig == NULL)
           {

                ///los primeros black es para "borrar las lineas y sobre escribir"
               setcolor(WHITE);
               line(auxL->x1+30,auxL->y1+L,auxL->x1+L,auxL->y1);///Dibuja particion si no tiene a donde apunta
           }
            auxL=auxL->sig;
            aux=aux->Der;
        }

        setcolor(WHITE);
        line((*cab)->x,(*cab)->y,(*cab2)->x1,(*cab2)->y1); ///Nos dibuja como nuestra cabereca apunta al primer nodo
        line((*cab2)->x1,(*cab2)->y1,aux2->x-10,aux2->y-10);
        line((*cab2)->x1,(*cab2)->y1,aux2->x+10,aux2->y-10);
    }
}
