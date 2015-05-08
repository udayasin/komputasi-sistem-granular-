# komputasi-sistem-granular-
tugas KSG 
#include <iostream>
#include <fstream>

using namespace std;

int main (int argc, char *argv[])
{
    int N = 10/0.005;
    ofstream gps ("particles.gps");
    gps << "#Execute: gnuplot particles.gps" << endl;
    gps << "#Set terminal to gif" << endl;
    gps << "set term gif size 1000, 500 animate delay " << 0.1 << endl;
    gps << "set output 'particles.gif' " << endl;
    gps << "set xrange [0:0.005]" << endl;
    gps << "set yrange [0:0.001]" << endl;

    gps << "input = \"data_code.txt\"" << endl;

    for (int i = 0; i <= N; i+=10)
    {
                gps << "plot input u "<<2<<":"<<3<<" every::"<<i<<"::"<<i<<" lt "<<2<<" lw "<<3<<" pt 7 ps 10 t\"\"" << endl;
    }
    gps.close();
    return 0;
}
