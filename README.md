# Group Project
Data Structure - Group Project - Splitwise

## Sub Topic
* Group - Member




TUBES1.H
#ifndef TUBES1_H_INCLUDED
#define TUBES1_H_INCLUDED

#include <string>
#include <stdlib.h>
#include <conio.h>
#include <iostream>

#define nil NULL
#define info(p) (p)->info
#define first(L) (L).first
//#define firstChild(C) (C).firstChild
//#define last_p(L) (L).last_p
#define next(p) (p)->next
#define last(L) (L).last
#define prev(p) (p)->prev
#define Child(p) (p)->Child
//#define last_p(L) (L).last_p
using namespace std;

typedef struct elm_parent *address;
typedef struct elm_child *address_child;

struct infotype_parent
{
    int id_grup;
    string nama_grup;
    string status;
    int jmlAnggota;
};

struct infotype_child{
    int id_anggota;
    string nama_anggota;
    string tglBergabung;
    int profit;
};

struct elm_parent {
    infotype_parent info;
    address next, prev;
    address_child Child;//adrgrup (&g); Child(g);
};

struct Parent {
    address first, last;
};

struct elm_child {
    infotype_child info;
    address_child next;
};

struct Child {
    //address_child firstChild;
};


void createList(Parent &L);
address alokasi_parent(infotype_parent x);
address_child alokasi_child(infotype_child x_c);
bool list_empty(Parent L);
bool list_empty_anak(address C);
void printinfo(Parent L);
void viewlist_child(address C);
void inputdata(infotype_parent *x);
void inputdata_child(infotype_child *x_c);

void insertFirst(Parent &L , address p);
void insertLast(Parent &L , address p);

void insertFirst_c(Parent &L,address p, address_child q);
void insertLast_c(Parent &L,address p, address_child q);

void deleteFirst(Parent &L, address &p);
void deleteLast(Parent &L, address &p);
void deleteFirst_c(Parent &L, address p, address_child &c);
void deleteLast_c(Parent &L, address p, address_child &c);

address findElm(Parent L,int x);
address_child findElm_child(Parent L,address p,int x_c);

void insertAfter(Parent &L, address p, address q);
void insertAfter_c(Parent &L,address p, address_child c, address_child z);

void deleteAfter(Parent &L, address &p, address q);
void deleteAfter_c(Parent &L, address p, address_child &c, address_child z);

void insertData(Parent &L, address c);
void dealokasi(address &p);
//void deleteAja(List &, address &);
//void rata2(List &, address );
void delete_p (Parent &L, address &p);
void edit_p (Parent &L,int x);
void edit_c (Parent &L,address &p,int x_c);
void sort_p (Parent &L);
void sort_c (Parent L,address &x);
int profit(address p);

#endif // TUBES1_H_INCLUDED


TUBES1.CPP

#include "tubes1.h"
using namespace std;

void insertFirst(Parent &L , address p)
{
    if(list_empty(L)) {
        first(L) = p;
        last(L) = p;
    } else {
        prev(first(L)) = p;
        next(p) = first(L);
        first(L) = p;
    }
}

address alokasi_parent(infotype_parent x){
    address p=new elm_parent;
    info(p)=x;
    next(p)=NULL;
    prev(p)=NULL;
    Child(p)=NULL;
    return p;
}

address_child alokasi_child(infotype_child x_c){
    address_child p=new elm_child;
    info(p)=x_c;
    next(p)=NULL;
    return p;
}

void dealokasi(address &p){
    delete p;
}

void insertLast(Parent &L , address p)
{
    if(list_empty(L)) {
        insertFirst(L, p);
    } else {
        next(last(L)) = p;
        prev(p) = last(L);
        last(L) = p;
    }
}

address_child findElm_child(Parent L,address p,int x_c){
    //address parent =new elm_parent;
    if(Child(p)!=NULL){
        address_child n=Child(p);
        while ((info(n).id_anggota!=x_c)&& (n!=nil)){
            n=next(n);
        }
        if (n!=nil){
            return n;
        }
        else
        {
            return 0;
        }
    }
    else{
        return 0;
    }
}
/*address_child findElm_child(Child L, infotype_child x_c){
    address parent=new elm_parent;
    address_child q = nil;
    if(!list_empty_anak(L)) {
        q = firstChild(parent->Child);
        while(q != nil) {
            if(info(q).id_anggota == x_c.id_anggota) {
                break;
            }
            else{
                q=next(q);
            }
            //p = next(p);
        }
    }
    return q;
}*/

void insertFirst_c(Parent &L,address p, address_child q)
{
    //address_child parent =new elm_child;
    infotype_child x_c=info(q);
    if(!list_empty(L)) {
        //address parent;//=findElm_child(L,p,x_c);
        if(Child(p)==NULL){
            Child(p) = q;
        } else {
            next(q) = Child(p);
            Child(p) = q;
        }
    }
}

void insertLast_c(Parent &L,address p, address_child q)
{   //address parent = new elm_parent;
    infotype_child x_c=info(q);
    if(!list_empty(L)) {
        //address parent = p; //= findElm_child(L,p,x_c);
        if(Child(p)==NULL) {
            insertFirst_c(L, p, q);
        } else {
            address_child r = Child(p);
            while(next(r) != NULL){
                r = next(r);
            }
            next(r) = q;
        }
    }
}
void insertAfter(Parent &L, address p, address q)
{
    if(list_empty(L)) {
        insertFirst(L, p);
    } else {
        next(p) = next(q);
        prev(p) = q;
        next(q) = p;
        prev(next(p)) = p;
    }
}
void insertAfter_c(Parent &L,address p, address_child c, address_child z)
{   //address parent = new elm_parent;
    infotype_child x_c=info(z);
    if(!list_empty(L)) {
        //address parent; //= findElm_child(L,p,x_c);
        if(Child(p)==NULL) {
            insertFirst_c(L, p, c);
        } else {
            next(c) = next(z);
            next(z) = c;
        }
    }
}

void createList(Parent &L){
    first(L)=NULL;
    last(L)=NULL;
}

bool list_empty(Parent L)
{
    if((first(L)==NULL) && (last(L)==NULL))
    {
        return true;
    }
    else
    {
        return false;
    }
}

bool list_empty_anak(address c)
{
    //address parent = new elm_parent;
    if(Child(c)==NULL)
    {
        return true;
    }
        else{

          return false;
        }
}
void deleteFirst(Parent &L, address &p){
    if(!list_empty(L)) {
        address current = first(L);
        if(next(current) == NULL) {
            p = first(L);
            first(L) = NULL;
            last(L) = NULL;
        } else {
            p = first(L);
            //address q=prev_p(first(L));
            first(L) = next(first(L));
            next(p) = NULL;
            //prev_p(p) = NULL;
            prev(first(L)) = NULL;
            //next_p(q)=first_p(L);
        }
    }
    else{
        cout<<"DATA KOSONG"<<endl;
    }
}
void deleteLast(Parent &L, address &p){
    if(!list_empty(L)) {
        address current = first(L);
        if(next(current) == NULL) {
            deleteFirst(L, p);
        } else {
            p = last(L);
            //address q=next_p(last_p(L));
            last(L) = prev(p);
            //next_p(p)=NULL;
            next(last(L)) = NULL;
            prev(p) = NULL;
            //prev_p(q)=last_p(L);
        }
    }
    else{
        cout<<"DATA KOSONG"<<endl;
    }
}


void deleteFirst_c(Parent &L, address p, address_child &c)
{
    //address parent = new elm_parent;
    infotype_child x_c=info(c);
        if(first(L)!=NULL)
            {
                //address parent; //= findElm_child(L,p,x_c);
                if(Child(p) != nil)
                {
                    if(next(Child(p)) == nil)
                    {
                        //c = Child(p);
                        Child(p) = nil;
                    }
                    else
                    {
                        c = Child(p);
                        Child(p) = next(Child(p));
                        next(c) = nil;
                    }
                }
            }
        else{
            cout<<"DATA PARENT KOSONG"<<endl;
        }
}

void deleteLast_c(Parent &L, address p, address_child &c)
{
        //address parent =new elm_parent;
        infotype_child x_c=info(c);
        if(first(L)!=nil)
        {
            if(Child(p) != nil)
            {
                if(next(Child(p))==NULL){
                    deleteFirst_c(L, p, c);
                }
                else{
                    address_child prec=Child(p);
                    while(next(next(prec))!=NULL){
                        prec=next(prec);
                    }
                    next(prec)=NULL;
                }
            }
        }
        else{
            cout<<"DATA PARENT KOSONG "<<endl;
        }
        /*else
            {
                while(next(next(c))!=NULL){
                    c=next(c);
                }
                //c= last(parent->firstChild);
                //last(parent->child) = prev(*c);
                next(c) = nil;
                //next_p(last(parent->firstChild)) = nil;
            }*/
}

void deleteAfter_c(Parent &L, address p, address_child &c, address_child prec)
{
//address parent=new elm_parent;
infotype_child x_c=info(c);
     /// ngecek list kosong apa nggak
    if(!list_empty(L)) {
        //address parent; //= findElm_child(L,p,x_c);
        /// ngecek list anak kosong apa nggak
        if(Child(p)!=nil) {
            /// ngecek apakah cuma ada 1 elemen di listanak
            /// kenapa di cek? karena udah beda algoritma
            if(next(Child(p)) == nil) {
                Child(p) = nil;
                //last(induk->lanak) = Nil;
            } else {
                address_child current = Child(p);
                /// kalo list anak gak kosong
                while(current != nil) {
                    if(current == prec) {
                        /// ngecek apakah setelah current masih ada minimal 2 elemen lain
                        /// kenapa di cek? karena udah beda algoritma
                        if(next(current) == nil) {
                            deleteFirst_c(L, p, c);//daleteFirst_c
                        }
                        else {
                            //c = next(current);
                            next(current) = next(c);
                            //prev(next(current)) = current;
                            next(c) = nil;
                            //prev(*c) = nil;
                        }
                        break;
                    }
                    current = next(current);
                }
            }
        }
    }
    else{
        cout<<"DATA PARENT KOSONG "<<endl;
    }
}

void inputdata_child(infotype_child &x_c)
{

    cout<<"Nama Anggota : ";
    //cin>>(*x_c).nama_anggota;
    getline(cin,(x_c).nama_anggota);
    cout<<endl<<"ID Anggota   : ";
    cin>>(x_c).id_anggota;
    cout<<endl<<"Tanggal Bergabung: ";
    cin>>(x_c).tglBergabung;
    cout<<endl<<"Profit: ";
    cin>>(x_c).profit;
}
void deleteAfter(Parent &L, address &p, address prec){
    /// ngecek list kosong apa nggak
    if(!list_empty(L)) {
        address current = first(L);
        /// ngecek apakah current adalah elemen terakhir
        /// kenapa di cek? karena udah beda algoritma
        if(next(current) == NULL) {
            deleteFirst(L, p);
        }
        else {
            while(current != NULL) {
                if(current == prec) {
                    /// ngecek apakah setelah current masih ada minimal 2 elemen lain
                    /// kenapa di cek? karena udah beda algoritma
                    if(next(next(current)) == NULL) {
                        deleteLast(L, p);
                    }
                    else {
                        //p = next(current);
                        next(current) = next(next(p));
                        prev(next(p)) = current;
                        next(p) = NULL;
                        prev(p) = NULL;
                    }
                    break;
                }
                current = next(current);
            }
        }
    }
    else{
        cout<<"DATA KOSONG"<<endl;
    }
}

address findElm(Parent L,int x){
   //address p=first(L);
   if(!list_empty(L)){
        address p=first(L);
        while ((info(p).id_grup!=x) && (p!=NULL)){
            p=next(p);
        }
        return p;
    }
    else{
        return 0;
    }
}

void edit_p (Parent &L,int x /*address &p*/){
    if(list_empty(L)){
       cout<<"data kosong"<<endl;
    }
    else{
        address p=findElm(L,x);
        if(p!=NULL){
            cout<<"EDIT DATA PADA GRUP DENGAN NAMA "<<info(p).nama_grup<<endl;
            cout<<"==========================";
            cout<<endl<<"edit :"<<endl;
            cout<<"ID_GRUP        : "; cin>>info(p).id_grup;
            cout<<endl<<"NAMA GRUP      : "; cin>>info(p).nama_grup;
            cout<<endl<<"JUMLAH ANGGOTA : "; cin>>info(p).jmlAnggota;
            cout<<endl<<"STATUS GRUP    : "; cin>>info(p).status;
            cout<<endl;
            //cout<<endl<<"JUMLAH ANGGOTA : "; cin>>info(p).jmlAnggota;
            getch();
        }
    }
}

void edit_c (Parent &L,address &p,int x_c){
    if (list_empty(L)){
        cout<<"DATA KOSONG "<<endl;
    }
    else{
        address_child q=Child(p);
        q=findElm_child(L,p,x_c);
        if(q!=NULL){
            cout<<"EDIT ANGGOTA DARI NAMA GRUP "<<info(p).nama_grup<<" DENGAN ID ANGGOTA "<<info(q).id_anggota<<endl;
            cout<<"EDIT : "<<endl;
            cout<<"ID ANGGOTA        : "; cin>>info(q).id_anggota;
            cout<<endl<<"NAMA ANGGOTA      : "; cin>>info(q).nama_anggota;
            cout<<endl<<"TANGGAL BERGABUNG : "; cin>>info(q).tglBergabung;
            cout<<endl<<"PROFIT            : "; cin>>info(q).profit;
            getch();
        }
        else{
            cout<<"DATA ID TIDAK DITEMUKAN! "<<endl;
        }
    }
}

void sort_p (Parent &L){
    if (first(L)!=NULL){
        address p=first(L);
        address q=first(L);
        Parent L2;
        createList(L2);
        do{
            while ((p!=NULL)&&(q!=NULL)){
                if(info(q).id_grup>info(p).id_grup){
                    q=p;
                }
                p=next(p);
            }
            //insertLast(L2,q);
            ///cari posisi q delete fisrt/last/after
            if(next(q)==NULL){
                deleteLast(L,q);
            }
            else if(q==first(L)){
                deleteFirst(L,q);
            }
            else{
                address prec=first(L);
                while (next(prec)!=q){
                    prec=next(prec);
                }
                deleteAfter(L,q,prec);
            }
            insertLast(L2,q);
            q=first(L);
            p=first(L);
        } while(first(L)!=NULL);
    L=L2;
    }
}


void sort_c(Parent L,address &x){
if(first(L)!=NULL){
    address_child p=Child(x);
    address_child q=Child(x);
    address tmp;
    //info(tmp)=info(x);
    Child(tmp)=NULL;
        do{
            while ((p!=NULL)&&(q!=NULL)){
                if(info(q).id_anggota>info(p).id_anggota){
                    q=p;
                }
                p=next(p);
            }
            //insertLast(L2,q);
            ///cari posisi q delete fisrt/last/after
            if(next(q)==NULL){
                deleteLast_c(L,x,q);
            }
            else if(q==Child(x)){
                deleteFirst_c(L,x,q);
            }
            else{
                address_child prec=Child(x);
                while (next(prec)!=q){
                    prec=next(prec);
                }
                deleteAfter_c(L,x,q,prec);
            }
            insertLast_c(L,tmp,q);
            q=Child(x);
            p=Child(x);
        } while(Child(x)!=NULL);
    x=tmp;
    }
}

void viewlist_child(address p){
    if(Child(p)!=NULL){
       cout<<"========================="<<endl;
       cout<<"DATA ANAK PADA NAMA: "<<info(p).nama_grup<<endl;
       address_child q=Child(p);
       cout<<"DATA PADA ID GRUP: "<<info(p).id_grup<<endl;
       cout<<"=========================="<<endl;
       while(q!=NULL){
            cout<<"ID ANGGOTA        : "<<info(q).id_anggota<<endl;
            cout<<"NAMA ANGGOTA      : "<<info(q).nama_anggota<<endl;
            cout<<"TANGGAL BERGABUNG : "<<info(q).tglBergabung<<endl;
            cout<<"PROFIT            : "<<info(q).profit<<endl;
            q=next(q);
       }
    }
    else{
        cout<<"data tidak ada";
    }
}

void printinfo(Parent L){
    if(first(L)!=NULL){
        address p=first(L);
        while(p!=NULL){
            cout<<"ID GRUP: "<<info(p).id_grup<<endl;
            cout<<"NAMA GRUP: "<<info(p).nama_grup<<endl;
            cout<<"JUMLAH ANGGOTA: "<<info(p).jmlAnggota<<endl;
            cout<<"STATUS: "<<info(p).status<<endl;
            //cout<<"JUMLAH ANGGOTA: "<<info(p).jmlAnggota<<endl;
            //cout<<endl;
            if(Child(p)!=NULL){
                viewlist_child(p);
            }
            else{
                cout<<"data anak NULL"<<endl<<endl;
            }
            p=next(p);
        }
    }
    else{
        cout<<"parent kosong"<<endl;
    }
}

int profit(address p){
    int l=0;
    address_child q=Child(p);
    if (Child(p)!=NULL){
        while(q!=NULL){
            l=info(q).profit+l;
            q=next(q);
        }
    }
    return l;
}


MAIN.CPP

#include <iostream>
#include "tubes1.h"
using namespace std;

//tanya gimana caranya biar klo gak ketemu hasilnya bukan error
///tanya gimana caranya buat sorting child
///gimana caranya biar gak force close klo gk ketemu hasil yang cocok

int main()
{
    char x; infotype_parent z; infotype_child c; int l,n;
    Parent L;
    createList(L);
    do{
    system("cls");
    cout <<"GRUP ANGGOTA"<<endl;
    cout<<"MENU: "<<endl;
    cout<<"1. INSERT DATA"<<endl;
    cout<<"2. DELETE DATA"<<endl;
    cout<<"3. FIND DATA"<<endl;
    cout<<"4. SORT DATA"<<endl;
    cout<<"5. HITUNG PROFIT"<<endl;
    cout<<"6. VIEW LIST "<<endl;
    cout<<"7. EDIT DATA "<<endl;
    cout<<"MASUKKAN INPUTAN: ";
    cin>> x;

    if (x=='1'){
        cout<<"MENU INSERT"<<endl;
        cout<<"1. INSERT GRUP"<<endl;
        cout<<"2. INSERT ANGGOTA"<<endl;
        cout<<"MASUKKAN PILIHAN : ";
        cin>>x;
        if(x=='1'){
            cout<<"MASUKKAN DATA: "<<endl;
            cout<<"ID GRUP        : "; cin>>z.id_grup;
            cout<<endl;
            cout<<"NAMA GRUP      : "; //cin>>z.nama_grup;
            cin.ignore();
            getline(cin,z.nama_grup);
            cout<<endl;
            cout<<"JUMLAH ANGGOTA : ";cin>>z.jmlAnggota;
            cout<<endl;
            cout<<"STATUS GRUP    : ";cin>>z.status;
            cout<<endl;
            address p=alokasi_parent(z);
            insertLast(L,p);
            getch();
            }
        if(x=='2'){
            if(first(L)==NULL){
                cout<<"data parent kosong"<<endl;
            }
            else{
                cout<<"MASUKKAN ID PARENT YANG MAU DIISI : ";
                cin>>l;
                address p=findElm(L,l);
                if (p!=NULL){
                    cout<<endl<<"ID ANGGOTA        : "; cin>>c.id_anggota;
                    cout<<endl<<"NAMA ANGGOTA      : "; cin>>c.nama_anggota;
                    cout<<endl<<"TANGGAL BERGABUNG : "; cin>>c.tglBergabung;
                    cout<<endl<<"PROFIT            : "; cin>>c.profit;
                    cout<<endl;
                    address_child n=alokasi_child(c);
                    insertLast_c(L,p,n);
                }
                else{
                    cout<<"id tidak ditemukan!"<<endl;
                }

          }
        }
    }
    else if(x=='2'){
        cout<<"MENU DELETE: "<<endl;
        cout<<"1. DELETE GRUP"<<endl;
        cout<<"2. DELETE ANGGOTA"<<endl;
        cout<<"MASUKKAN MENU DELETE YANG DIPILIH: "; cin>>x;
        if(x=='1'){
            cout<<"MASUKKAN ID GRUP YANG INGIN DIDELETE: ";
            int a;
            cin>>a;
            address p=findElm(L,a);
            if (p!=NULL){
                if(next(p)==NULL){
                    deleteLast(L,p);
                }
                else if(p==first(L)){
                    deleteFirst(L,p);
                }
                else {
                    address prec=first(L);
                    while (next(prec)!=p){
                        prec=next(prec);
                    }
                    deleteAfter(L,p,prec);
                }
                cout<<"DATA TELAH DIHAPUS"<<endl;
            }
            else{
                cout<<"ID GRUP TIDAK DITEMUKAN"<<endl;
            }
            cout<<endl;
            getch();
        }
        if(x=='2'){
            cout<<"MASUKKAN ID ANGGOTA YANG INGIN DI DELETE: ";
            int a;
            cin>>a;
            address p=first(L);
            if (p!=NULL){
            do{
                if(Child(p)!=NULL){
                    address_child n=findElm_child(L,p,a);
                    if (n!=NULL){
                        if(next(n)==NULL){
                            deleteLast_c(L,p,n);
                        }
                        else if(n==Child(p)){
                            deleteFirst_c(L,p,n);
                        }
                        else{
                            address_child prec=Child(p);
                            while(next(prec)!=n){
                                prec=next(prec);
                            }
                            deleteAfter_c(L,p,n,prec);
                        }
                        cout<<"DATA ANGGOTA TELAH DIHAPUS "<<endl;
                    }
                    /*else{
                        cout<<"DATA TIDAK DITEMUKAN"<<endl;
                    }
                    break;*/
                }
                p=next(p);
            } while(p!=NULL);
        }
        else{
            cout<<"DATA KOSONG "<<endl;
        }
    }
}
    else if(x=='3'){
        cout<<"PILIH MENU FIND: "<<endl;
        cout<<"1. FIND GRUP "<<endl;
        cout<<"2. FIND ANGGOTA "<<endl;
        cout<<"MASUKKAN PILIHAN MENU : ";
        cin>>x;
        if(x=='1'){
            cout<<"INPUT ID GRUP YANG INGIN DICARI: "; cin>>l;
            address p=findElm(L,l);
            if (p!=NULL){
                cout<<"ID GRUP        : "<<info(p).id_grup<<endl;
                cout<<"NAMA GRUP      : "<<info(p).nama_grup<<endl;
                cout<<"JUMLAH ANGGOTA : "<<info(p).jmlAnggota<<endl;
                cout<<"STATUS GRUP    : "<<info(p).status<<endl;
                cout<<endl<<"DATA DITEMUKAN "<<endl;
            }
            else{
                cout<<"DATA KOSONG "<<endl;
            }
        }
        else if(x=='2'){
            cout<<"INPUT ID GRUP YANG AKAN DICARI: "; cin>>l;
            address p=findElm(L,l);
            if(p!=NULL){
                cout<<"MASUKKAN ID ANGGOTA YANG INGIN DICARI DARI GRUP "<<info(p).nama_grup<<" : ";
                cin>>l;
                address_child q=findElm_child(L,p,l);
                if(q!=NULL){
                    cout<<endl<<"ID ANGGOTA        : "<<info(q).id_anggota;
                    cout<<endl<<"NAMA ANGGOTA      : "<<info(q).nama_anggota;
                    cout<<endl<<"TANGGAL BERGABUNG : "<<info(q).tglBergabung;
                    cout<<endl<<"PROFIT            : "<<info(q).profit;
                    cout<<endl;
                }
                else{
                    cout<<"DATA KOSONG"<<endl;
                }
            }
            else{
                cout<<"DATA KOSONG"<<endl;
            }
        }
    }
    else if(x=='4'){
        cout<<"MENU SORT: "<<endl;
        cout<<"1. SORT GRUP"<<endl;
        cout<<"2. SORT ANGGOTA"<<endl;
        cout<<"PILIH MENU SORTING: ";
        cin>>x;
        if (x=='1'){
            sort_p(L);
            printinfo(L);
        }
        else if(x=='2'){
            cout<<"INPUT ID PARENT YANG CHILDNYA MAU DIURUTKAN: ";
            cin>>l;
            address p=findElm(L,l);
            if(p!=NULL){
                sort_c(L,p);
                cout<<"LIST ANGGOTA DARI GRUP "<<info(p).nama_grup<<" : "<<endl;
                viewlist_child(p);
        }
    }
}
    else if(x=='5'){
        cout<<"MASUKKAN ID PARENT YANG INGIN DIHITUNG PROFITNYA: ";
        cin>>l;
        address p=findElm(L,l);
        if (p!=NULL){
            n=profit(p);
            cout<<"PROFIT UNTUK GRUP DENGAN NAMA "<<info(p).nama_grup<<" ADALAH "<<n<<endl;
        }
    }
    else if(x=='6'){
        printinfo(L);
    }
    else if(x=='7'){
        cout<<"MENU EDIT "<<endl;
        cout<<"1. EDIT GRUP "<<endl;
        cout<<"2. EDIT ANGGOTA "<<endl;
        cout<<"MASUKKAN PILIHAN MENU: ";
        cin>>x;
        if (x=='1'){
            cout<<"MASUKKAN ID YANG INGIN DIEDIT: "; cin>>l;
            address p=findElm(L,l);
            if (p!=NULL){
                edit_p(L,l);
            }
            else{
                cout<<"DATA KOSONG "<<endl;
            }
        }
        else if(x=='2'){
            cout<<"MASUKKAN ID ANGGOTA YANG INGIN DI EDIT: "; cin>>l;
            address p=first(L);
            do{
                if (Child(p)!=NULL){
                    edit_c(L,p,l);
                    break;
                }
            p=next(p);
            } while(p!=NULL);
        }
    }
    else{
        cout<<"hello world"<<endl;
    }
    cout<<"apakah ingin lanjut? (y/n)";
    cin>>x;
    } while(x!='n');
    return 0;
}
