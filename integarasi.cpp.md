# komputasi-sistem-granular-
tugas KSG 
#pragma once

#include "Integration.h"

ParticleState EulerIntegration::integrate( ParticleState* currentState, Vector2 sigmaF, double deltaT )
{
	// Acceleration
	Vector2 a = sigmaF/currentState->Mass*deltaT;
	// Velocity
	Vector2 v = currentState->Velocity + a*deltaT;
	// Position
	Vector2 r = currentState->Position + v*deltaT;

	// Angular acceleration
	double alpha = sigmaF.x*currentState->Radius/(2.0f/5.0f*currentState->Mass*currentState->Radius*currentState->Radius);
	// Angular velocity
	double omega = currentState->Omega + alpha*deltaT;
	// Angle
	double rotation = currentState->Rotation + omega*deltaT;

	// Return updated state of the particle
	ParticleState updatedState = *currentState;
	updatedState.Acceleration = a;
	updatedState.Velocity = v;
	updatedState.Position = r;
	updatedState.Alpha = alpha;
	updatedState.Omega = omega;
	updatedState.Rotation = rotation;
	return updatedState;
}
