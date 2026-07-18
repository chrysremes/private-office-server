# Main Objective

Do a migration plan to upgrade acess of files from local access only to remote access for remote colaboration;

# Especific Objectives

1. Do this migration in the cheapest way;
2. Do this migration considering a physical machine to be a server, hosted in Borrazópolis;
3. Do this migration in the easiest way in terms of maintenance possible; 
4. Ensure that the server files (like word docs or excel spreadsheets) can be accessed by Windows machines, either through local network (from Borrazópolis) or through remote network / internet (from Marilândia do Sul);
5. Conflict management of files can continue to be done manually, as is;
6. The database shall continue to run in an Excel Spreasheet ("Cadastro"), as is;
7. Employees will continue to copy and paste information manually, to ensure business continuity and avoid disruptions;
8. The system shall have a response time similar to the one observed today for local users accessing files from Windows to Windows;
9. Remote response time shall be fast enough, but it is expected to be less then local response time;
10. Configure both local and remote networks to access the server files like an external mapped drive;

# Constraints

1. the business does not have buget for singing up Cloud Services (like Azure VMs) - we estimated a total of BRL 12k per year;
2. the business does not have budget for singing up a Microsoft 365 Business Standard Plan - we estimated a total of BRL 16k per year;
3. Using Microsoft 365 Basic with apps like Excel through Web browser is cheaper but not possible, since the spreadsheet database "Cadastro" is complex and does not work well with Microsoft apps in web versions;
4. the business does not have bugdet for a Windows Server license;
5. There is no IT department/guys to look regularly on this server/system, so, for example, updates must be handled automatically whenever possible;