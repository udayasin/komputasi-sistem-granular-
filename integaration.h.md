# komputasi-sistem-granular-
tugas KSG 
#pragma once

#include "Vector2.h"
#include "Particle.h"

// Provides abstract interface for different integration
// methods (Euler, Verlet, etc ). Implementation provided
// by its subclasses. This allows easily changing integration
// methods from outside the Particle class.
class Integration
{
public:
	virtual ParticleState integrate ( ParticleState* currentState, Vector2 sigmaF, double deltaT ) = 0;
};

// This class implements Euler integration
class EulerIntegration : public Integration
{
	ParticleState integrate( ParticleState* currentState, Vector2 sigmaF, double deltaT );
};

// This class implements Verlet integration
class VerletIntegration : public Integration
{
	ParticleState integrate( ParticleState* currentState, Vector2 sigmaF, double deltaT );
};
