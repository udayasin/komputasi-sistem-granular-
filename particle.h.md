# komputasi-sistem-granular-
tugas KSG 
#pragma once

#include "Particle.h"
#include "Integration.h"
#include "Plane.h"

void Particle::update( Vector2 sigmaF, Integration* integration, double deltaT )
{
	ParticleState newState = integration->integrate( &m_State, sigmaF, deltaT );
	m_State = newState;
}

Vector2 Particle::calculateForce( Plane* other )
{
	// friction force
	double force;
	if (other->getSurfaceVelocity()>0.0f)
		force = other->m_MuK*m_State.Mass*9.8f;
	else
		force = -(other->m_MuK*m_State.Mass*9.8f);
	// total acting on body
	double sigmaF = m_State.Mass*other->m_Frequency*other->m_Frequency*other->m_Position;
	return Vector2(sigmaF,0.0f);
}

void Particle::integrate( Plane* plane, Integration* integration, double time, double deltaT )
{
	// Plane's frequency and amplitude
	double w = plane->m_Amplitude;
	double A = plane->m_Frequency;
	// Gravity
	double g = 9.8f;
	// Acceleration
	double ax = (m_State.Mass*w*(2*PI*w)*(2*PI*w)*cos(2*PI*w*time)-plane->m_MuK*m_State.Mass*g)/m_State.Mass;
	Vector2 a = Vector2(ax, 0.0f);
	// Velocity
	Vector2 v = m_State.Velocity + a*deltaT;
	// Position
	Vector2 r = m_State.Position + v*deltaT;

	// Angular acceleration
	double alpha = plane->m_MuK*g*2/(5*m_State.Radius);
	// Angular velocity
	double omega = m_State.Omega + alpha*deltaT;
	// Angle
	double rotation = m_State.Rotation + omega*deltaT;

	// Update state of the particle
	m_State.Velocity = v;
	m_State.Position = r;
	m_State.Omega = omega;
	m_State.Rotation = rotation;
}
