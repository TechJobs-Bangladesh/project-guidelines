Project Conventions & Documentation Guidelines
________________________________________
1. Naming Conventions
•	Classes, Properties, Methods: Use PascalCase.
Examples: JobApplication, JobTitle, CreateJob()
•	Variables, Parameters: Use camelCase.
Examples: jobId, userProfile
•	Interfaces: Prefix with I.
Examples: IJobRepository, IUserService
•	Enums: Use PascalCase for both enum types and values.
Example: JobStatus.Open
________________________________________
2. Folder and Project Structure
•	Layered Architecture:
________________________________________
I. TechJobs.Domain
Purpose: Core business models, enums, and interfaces that are totally independent of any framework.
Includes:
•	Entities (e.g., User, Job, Application)
•	Enums (UserStatus, JobType)
•	Business interfaces (e.g., IUser, not repositories/services)
•	Business rules (if any)
________________________________________
II. TechJobs.Application
Purpose: Business logic, services, use cases, DTOs, service interfaces.
Includes:
•	Service interfaces (e.g., IUserService)
•	Service implementations (e.g., UserService)
•	DTOs (Data Transfer Objects, e.g., UserDto)
•	Application logic (e.g., validation, business rules)
•	Contracts between layers
References:
•	Reference TechJobs.Domain
________________________________________
III. TechJobs.Infrastructure
Purpose: Data access, repository implementations, EF Core context, third-party integrations.
Includes:
•	EF Core DbContext (TechJobsDbContext)
•	Repository interfaces (IGenericRepository<T>, IUserRepository)
•	Repository implementations (GenericRepository<T>, UserRepository)
•	Unit of Work (IUnitOfWork, UnitOfWork)
•	Any code that talks to databases, files, or external systems
References:
•	Reference TechJobs.Domain and TechJobs.Application
________________________________________
IV. TechJobs.API
Purpose: HTTP API (Controllers, filters, startup/configuration, main entry point)
Includes:
•	Controllers (e.g., UsersController)
•	API filters (e.g., exception handling)
•	Startup configuration (Program.cs or Startup.cs)
•	Model binding, request/response formatting
References:
•	Reference TechJobs.Application (to use services, DTOs)
•	Reference TechJobs.Infrastructure (for DI setup, migrations, but keep logic in Application/Domain)
________________________________________
5. TechJobs.Common (Optional, for shared code)
Purpose: Shared utilities, helpers, constants.
Includes:
•	Helper classes (PasswordHasher)
•	Utilities (DateTimeHelper)
•	Constants
References:
•	Can be referenced by any other project as needed.
________________________________________
Folder Structure Example (per project)
TechJobs.Domain
Code
/Entities
    User.cs
    Job.cs
/Enums
    UserStatus.cs
    JobType.cs
/Interfaces
    IUser.cs
TechJobs.Application
Code
/DTOs
    UserDto.cs
/Services
    IUserService.cs
    UserService.cs
TechJobs.Infrastructure
Code
/Repositories
    IGenericRepository.cs
    GenericRepository.cs
    IUserRepository.cs
    UserRepository.cs
/UnitOfWork
    IUnitOfWork.cs
    UnitOfWork.cs
/Data
    TechJobsDbContext.cs
TechJobs.API
Code
/Controllers
    UsersController.cs
    JobsController.cs
/Filters
    ExceptionFilter.cs
Program.cs
TechJobs.Common (if used)
Code
PasswordHasher.cs
DateTimeHelper.cs
________________________________________
3. Commenting and Documentation (Google it in case of any confusion)
•	Summary XML Comments:
Use /// <summary>, /// <param>, and /// <returns> for public classes, methods, and properties.
Example:
/// <summary>
/// Creates a new job posting.
/// </summary>
/// <param name="jobDto">The job details as DTO.</param>
/// <returns>The ID of the created job.</returns>
public int CreateJob(JobDto jobDto)
{
    // ...
}
•	File Header Comments (Optional): Include author, date, and purpose at the top of each file, if desired.
Code
// -----------------------------------------------------------------------------
// File: JobController.cs
// Author: Sayeed
// Created: 2025-06-20
// Purpose: API controller for job-related endpoints.
// -----------------------------------------------------------------------------
•	Inline Comments:
Use sparingly, only for clarifying complex or non-obvious logic. Prefer self-explanatory code and method names.
________________________________________
4. Example Controller
Code
/// <summary>
/// Handles job-related API requests.
/// </summary>
[ApiController]
[Route("api/v1/[controller]")]
public class JobsController : ControllerBase
{
    /// <summary>
    /// Gets all jobs.
    /// </summary>
    /// <returns>List of jobs.</returns>
    [HttpGet]
    public async Task<IActionResult> GetJobs()
    {
        // ...
        return Ok(new ApiResponse<List<JobDto>>(jobs));
    }
}
________________________________________

