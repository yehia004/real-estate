Real Estate Platform - Technical Documentation
-- Project Overview
A scalable real estate marketplace designed to connect property seekers with owners. This document outlines the Low-Level Design (LLD) and the architectural decisions made during the initial development phase.

-- Tech Stack
Frontend: React (Vite)

Backend: Node.js (Express) / Supabase

Database: PostgreSQL

Authentication: JWT with bcrypt hashing

-- Feature Design & API Specifications
1. Property Management
Feature	Endpoint	Method	Description
List Properties	/api/properties	GET	Fetch all available listings with pagination.
Property Details	/api/properties/:id	GET	Retrieve full data for a single property.
Search/Filter	/api/properties?params	GET	Filter by city, price range, rooms, and type.
2. User Interactions
Authentication: Secure registration and login flow using JWT.

Favorites: A many-to-many relationship allowing users to bookmark listings.

Contact System: Direct inquiry pipeline connecting users to property owners.

Property Views: A logging system to track engagement (optimized for high-frequency writes).

--Database Schema (ERD Snippet)
The system utilizes a relational PostgreSQL structure for data integrity:

Users Table: id, name, email, password_hash, created_at.

Properties Table: id, owner_id (FK), title, price, city, type, etc.

PropertyViews Table: id, property_id (FK), viewer_id (FK), viewed_at.

Favorites Table: id, user_id (FK), property_id (FK).

-- Strategic Decisions (Senior Insights)
--- REST vs. WebSockets
For the Property Views feature, REST was chosen over WebSockets.

Reasoning: Views are a "fire-and-forget" action. Opening a persistent WebSocket connection for a single log entry is an unnecessary drain on server resources. REST provides a stateless, scalable standard that is easier to cache and monitor.

--- Near Real-Time vs. Real-Time
The "Who viewed my property" feature is implemented as Near Real-Time.

---Reasoning: From a Business/UX perspective, users check their insights periodically via notifications or dashboard visits. Avoiding a true Real-Time (Live) implementation reduces infrastructure costs and complexity without sacrificing user value.

---- Authorization Logic
Strict server-side checks are implemented to ensure data privacy:

JavaScript
// Example: Only the owner can edit or see detailed analytics
if (request.user.id !== property.owner_id) {
  return response.status(403).json({ error: "Unauthorized" });
}
-- Roadmap & Priorities
Phase 1: Core Listing & Discovery (MVP)

Phase 2: Authentication & User Personalization (Favorites)

Phase 3: Communication Layer (Contact Owner)

Phase 4: Analytics & Insights (Property Views)
