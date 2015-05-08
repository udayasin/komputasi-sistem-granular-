# komputasi-sistem-granular-
tugas KSG 
// Granular1_console.cpp : Defines the entry point for the console application.
//

#include "stdafx.h"
#include <iostream>
#include <conio.h>
#include <fstream>

#include "Particle.h"
#include "Plane.h"
#include "Integration.h"

using namespace std;

int _tmain(int argc, _TCHAR* argv[])
{
	// Choose euler as our integration method
	Integration* pIntegration = new EulerIntegration();

	// Create one particle and the oscillating plane
	Particle particle( 0.01f, 0.001f, 0, Vector2(0.0f), Vector2(0.0f), 0.0f, 0.0f );
	Plane plane( 5.0f, 0.02f, 0.0005 );

	double simulationPeriod = 10.0f; // simulate for 10 seconds
	double deltaT = 0.005f;
	double time = 0.0f; // current time for every step

	// Open file
	char ofn[] = "data08.txt";
	ofstream fout;
	fout.open(ofn);

	if(fout.is_open())
	{
		fout << "t \t\tx \t\ty \t\tvx \t\tax \t\ttheta \t\tomega \t\talpha" << endl;

		// Main loop
		while( time <= simulationPeriod )
		{
			// Update the oscillating plane
			plane.update(time);
			// Calculate force acted on particle by the plane
			Vector2 sigmaF = particle.calculateForce( &plane );
			// Update the particle
			particle.update( sigmaF, pIntegration, deltaT );

			// Print out the data
			fout<<time<<"\t\t";
			fout<<particle.m_State.Position.x*100.0f<<"\t\t";
			fout<<particle.m_State.Position.y*100.0f<<"\t\t";
			fout<<particle.m_State.Velocity.x*100.0f<<"\t\t";
			fout<<particle.m_State.Acceleration.x*100.0f<<"\t\t";
			fout<<particle.m_State.Rotation<<"\t\t";
			fout<<particle.m_State.Omega<<"\t\t";
			fout<<particle.m_State.Alpha<<endl;

			// Advance time
			time += deltaT;
		}

		fout.close();
	}

	return 0;
}

