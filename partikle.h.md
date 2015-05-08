# komputasi-sistem-granular-
tugas KSG 
#pragma once

#include "Vector2.h"

class Integration;
class Plane;

// State of a particle at one moment in time
struct ParticleState
{
	// Particle radius in meters
	double Radius;
	// Mass in kilograms
	double Mass;
	// Moment of inertia in kgm^2
	double MOI;

	// 2-D position (x,y)
	Vector2 Position;
	// 2-D velocity (vx,vy)
	Vector2 Velocity;
	// 2-D acceleration (ax, ay)
	Vector2 Acceleration;
	// Counter-clockwise angle in radians
	double Rotation;
	// Counter-clockwise angular velocity in radians/s
	double Omega;
	// Counter-clockwise angular acceleration in radians/s^2
	double Alpha;
};

// 2-D particle class
class Particle
{
public:
	// Default constructor initializes particle at position (0,0)
	// with zero initial velocity and no rotation.
	// Defaults to 0.01m radius, 0.01kg mass, and 0.01kgm^2 MOI
	inline Particle()
	{
		m_State.Radius = 0.01f;
		m_State.Mass = 0.01f;
		m_State.MOI = 0.01f;

		m_State.Position = Vector2(0,0);
		m_State.Velocity = Vector2(0,0);
		m_State.Acceleration = Vector2(0,0);
		m_State.Rotation = 0.0f;
		m_State.Omega = 0.0f;
		m_State.Alpha = 0.0f;
	}
	// Initialize with parameters
	inline Particle( double radius, double mass, double momentOfInertia, Vector2 position, Vector2 velocity, double rotation, double omega )
	{
		m_State.Radius = radius;
		m_State.Mass = mass;
		m_State.MOI = momentOfInertia;

		m_State.Position = position;
		m_State.Velocity = velocity;
		m_State.Acceleration = Vector2(0,0);;
		m_State.Rotation = rotation;
		m_State.Omega = omega;
		m_State.Alpha = 0.0f;
	}

	// Calculate interaction force between this and another particle
	Vector2 calculateForce( Particle* other );

	// Calculate interaction force between this particle and a plane
	Vector2 calculateForce( Plane* other );

	/*
	Update the particle 
	@param
		sigmaF - accumulated force by all interactions
	@param
		integration - concrete subclass of Integration (ie. EulerIntegration),
		implementing the integration method. This allows easily changing integration
		methods from outside this class.
	@param
		deltaT in seconds
	*/
	void update( Vector2 sigmaF, Integration* integration, double deltaT );

	// Special case: integrate with only 1 plane interaction
	void integrate( Plane* plane, Integration* integration, double time, double deltaT );

public:
	// Current state of the particle
	ParticleState m_State;
};
