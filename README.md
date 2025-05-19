<html lang="pt-BR">
<head>
<meta charset="utf-8"/>
<meta content="width=device-width, initial-scale=1.0" name="viewport"/>
<title>Gerenciador de Notas de Boletim - MÉDIOTEC</title>
<style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; /* Modern font */
            margin: 0;
            background-color: #f0f2f5; /* Slightly gray background */
            color: #333;
            min-height: 100vh;
            padding-bottom: 40px; /* Add bottom padding for fixed footer */
            box-sizing: border-box; /* Include padding in total height */
        }
        /* Center login container when visible */
        body:has(#loginContainer:not(.hidden)) {
            display: flex;
            justify-content: center;
            align-items: center;
            background: linear-gradient(to bottom right, #005aaa, #00397a); /* Gradient background for login */
            padding-bottom: 0; /* Remove body padding on login screen */
        }

        h1, h2, h3 {
            color: #1a73e8; /* Blue */
        }
        h2, h3 {
            border-bottom: 2px solid #ff6600; /* Orange */
            padding-bottom: 5px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: center;
        }
        th {
            background-color: #1a73e8; /* Blue */
            color: white;
        }
        .button {
            background-color: #ff6600; /* Orange */
            color: white;
            padding: 10px 15px;
            border: none;
            cursor: pointer;
            margin: 5px;
            border-radius: 5px;
            display: block;
            width: calc(100% - 10px);
            transition: background-color 0.3s ease; /* Smooth transition */
        }
        .button:hover {
            background-color: #ff4500; /* Darker orange */
        }
         .button:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }
         .button.delete-button {
             background-color: #dc3545; /* Red for delete */
         }
         .button.delete-button:hover {
             background-color: #c82333; /* Darker red */
          }
        .hidden {
            display: none !important;
        }
        .error {
            color: #dc3545; /* Bootstrap red */
            font-size: 14px;
            margin-top: 10px; /* Space above */
        }

        /* --- Improved Login Screen Styles --- */
        #loginContainer {
            text-align: center;
            padding: 40px 35px; /* More padding */
            border: none; /* Remove border */
            border-radius: 10px; /* More rounded corners */
            background-color: #ffffff; /* White background */
            position: relative;
            overflow: hidden;
            width: 400px; /* Slightly larger container */
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15); /* More pronounced shadow */
        }
        /* Highlighted SENAC name */
        #loginContainer .senac-title {
            font-size: 3em; /* Larger size */
            font-weight: bold;
            color: #ff6600; /* Changed to orange */
            margin-bottom: 30px; /* More space below */
            line-height: 1.1;
        }

        /* Login Watermark Logo (Adjusted) */
        .login-watermark {
            position: absolute;
            bottom: 10px; /* Bottom position */
            left: 50%;
            transform: translateX(-50%);
            opacity: 0.05; /* More subtle */
            z-index: 0;
            pointer-events: none;
            width: 60%; /* Relative width */
        }
        .login-watermark img {
             max-width: 100%;
             height: auto;
        }

        /* Improved Input Fields */
        #loginContainer input[type="text"],
        #loginContainer input[type="password"] {
            padding: 15px; /* More internal padding */
            margin: 15px 0; /* More vertical space */
            width: 100%;
            border-radius: 5px;
            border: 1px solid #ccc; /* Subtle border */
            box-sizing: border-box;
            font-size: 16px;
            transition: border-color 0.3s ease, box-shadow 0.3s ease; /* Transitions */
        }
        #loginContainer input[type="text"]:focus,
        #loginContainer input[type="password"]:focus {
            border-color: #ff6600; /* Orange on focus */
            box-shadow: 0 0 0 3px rgba(255, 102, 0, 0.2); /* Outer shadow on focus */
            outline: none; /* Remove default outline */
        }

         /* Improved Login Button */
         #loginContainer #loginButton {
             width: 100% !important; /* Full width */
             display: block !important;
             padding: 15px 30px; /* Generous padding */
             margin-top: 25px; /* Space above */
             font-size: 18px; /* Font size */
             font-weight: bold;
             background-color: #ff6600; /* Orange */
             border: none;
             border-radius: 5px;
             color: white;
             cursor: pointer;
             transition: background-color 0.3s ease, box-shadow 0.3s ease;
         }
         #loginContainer #loginButton:hover {
             background-color: #ff4500; /* Darker orange */
             box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
         }
         /* --- End Login Styles --- */


        #appContainer {
            display: flex;
            width: 100%;
            height: 100vh;
            overflow: hidden;
            background-color: #fff; /* White background for app */
        }

        #sidebar {
            width: 230px;
            background-color: #f8f9fa; /* Bootstrap sidebar color */
            padding: 15px;
            display: flex;
            flex-direction: column;
            border-right: 1px solid #dee2e6; /* Bootstrap border */
            height: 100%;
            box-sizing: border-box;
            flex-shrink: 0;
        }
        #sidebar h2, #sidebar h3 {
            margin-top: 10px;
            margin-bottom: 10px;
            font-size: 1.1em; /* Adjusted size */
            color: #495057; /* Darker color */
        }
        #sidebar .button {
            margin-bottom: 10px;
             text-align: left; /* Align button text */
             padding: 10px 12px;
             width: 100%; /* Full width */
             box-sizing: border-box;
        }
        #sidebar .button.logout-button {
            margin-top: auto;
            background-color: #6c757d; /* Dark gray logout */
        }
         #sidebar .button.logout-button:hover {
             background-color: #5a6268;
         }

        #mainContentArea {
            flex-grow: 1;
            padding-left: 10px;
            padding-right: 10px;
            padding-top: 80px;
            padding-bottom: 25px;
            overflow-y: auto;
            height: calc(100vh - 80px - 40px);
            box-sizing: border-box;
            position: relative;
        }
        /* App Text - Top Right Corner */
        #appHeaderText {
            position: fixed;
            top: 15px;
            right: 25px;
            font-size: 2em; /* Large size */
            font-weight: bold;
            color: #ff6600; /* Changed to orange */
            z-index: 1001;
        }


        #mainContentArea h1 {
            margin-top: 0;
            margin-bottom: 25px; /* More space below main title */
        }

        /* General adjustments for inputs/selects within the App */
        input[type="text"], input[type="password"], select {
            padding: 10px; /* Increase padding */
            margin: 5px 3px; /* Adjust margin */
            width: auto; /* Allow initial auto adjustment */
            min-width: 180px; /* Minimum width */
            border-radius: 5px;
            border: 1px solid #ced4da; /* Bootstrap border */
            box-sizing: border-box;
            font-size: 15px;
        }
         input[type="text"]:disabled, select:disabled {
             background-color: #e9ecef;
             cursor: not-allowed;
         }
        /* Inline buttons with inputs */
        #addStudentSection button, #addDisciplineSection button, #mainContentArea h2 + button, #mainContentArea h3 + button,
        #printControls button /* Style for new print buttons */
         {
             width: auto !important;
             display: inline-block !important;
             vertical-align: middle; /* Align with selects/inputs */
             margin-left: 10px;
        }
        /* Adjustment for observation input, can be wider */
        #addDisciplineSection input[type="text"]#observationInput {
            min-width: 250px; /* Allow a bit more space for observation */
        }


        /* Modal styles */
        .modal { position: fixed; left: 0; top: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.5); z-index: 1000; display: flex; justify-content: center; align-items: center; }
        .modal-content { background-color: white; padding: 20px; border-radius: 8px; width: 500px; max-width: 90%; max-height: 80vh; overflow-y: auto; position: relative; z-index: 1002; }
        .modal-content h2 { margin-top: 0; }
        .modal-content label{ display: block; margin-top: 10px; font-weight: bold; }
        .modal-content input[type="text"], .modal-content input[type="password"] { width: 100%; } /* Inputs in modal 100% */
        .close-button { position: absolute; top: 10px; right: 15px; font-size: 24px; font-weight: bold; cursor: pointer; }
        #professorDisciplinesCheckboxes div, #professorClassesCheckboxes div, #editProfessorDisciplinesCheckboxes div, #editProfessorClassesCheckboxes div { margin-bottom: 5px; } /* Added edit IDs */
        #professorDisciplinesCheckboxes label, #professorClassesCheckboxes label, #editProfessorDisciplinesCheckboxes label, #editProfessorClassesCheckboxes label { margin-left: 5px; font-weight: normal; display: inline-block; margin-right: 10px; } /* Added edit IDs */
        #professorDisciplinesCheckboxes input[type="checkbox"], #professorClassesCheckboxes input[type="checkbox"], #editProfessorDisciplinesCheckboxes input[type="checkbox"], #editProfessorClassesCheckboxes input[type="checkbox"] { vertical-align: middle;} /* Added edit IDs */

         /* Specific styles for discipline/class assignment checkboxes */
        .discipline-assignment {
            margin-bottom: 15px;
            padding: 10px;
            border: 1px solid #eee;
            border-radius: 5px;
            background-color: #f9f9f9;
        }
        .discipline-assignment h4 {
            margin-top: 0;
            margin-bottom: 8px;
            color: #1a73e8;
            font-size: 1.1em;
        }
        .discipline-assignment .class-checkboxes label {
            margin-right: 15px;
             font-weight: normal;
        }


        /* Style for editable cells */
        .editable-cell {
            cursor: pointer;
            border: 1px dashed #ccc; /* Dashed border to indicate editable */
        }
         .editable-cell:hover {
             background-color: #e9e9e9;
         }
         .editable-cell input[type="text"], .editable-cell select {
             width: 100%;
             padding: 0;
             margin: 0;
             border: none;
             background: none;
             text-align: center;
             font-size: inherit;
             font-family: inherit;
             box-sizing: border-box;
             outline: none; /* Remove outline when editing */
         }
         .editable-cell.editing {
             background-color: #fff; /* White background when editing */
         }

        /* Styles for User Management Table */
        #manageUsersSection table {
             margin-top: 15px;
             width: 100%;
        }
         #manageUsersSection table th, #manageUsersSection table td {
             text-align: left;
              padding: 10px; /* More padding */
         }
          #manageUsersSection table td:nth-last-child(3) { /* Disciplines Assigned Column (third to last before Actions) */
             font-size: 0.9em;
             overflow-wrap: break-word; /* Break text if too long */
          }
           #manageUsersSection table td:nth-last-child(2) { /* Classes Assigned Column (second to last before Actions) */
             font-size: 0.9em;
             overflow-wrap: break-word; /* Break text if too long */
          }
           #manageUsersSection table th:nth-last-child(3), #manageUsersSection table td:nth-last-child(3) {
              text-align: center; /* Center Disciplines title and content */
           }
           #manageUsersSection table th:nth-last-child(2), #manageUsersSection table td:nth-last-child(2) {
              text-align: center; /* Center Classes title and content */
           }
          #manageUsersSection table td:nth-last-child(1) { /* Password Column */
             font-family: 'Courier New', Courier, monospace; /* Monospaced font for passwords */
             font-size: 0.9em;
             overflow-wrap: break-word; /* Break password if too long */
          }
          #manageUsersSection table th:nth-last-child(1), #manageUsersSection table td:nth-last-child(1) { /* Center Password title */
             text-align: center;
          }
           #manageUsersSection table td:last-child { /* Actions Column */
                text-align: center;
                width: 120px; /* Space for buttons */
           }
           #manageUsersSection table .button {
                width: auto;
                display: inline-block;
                margin: 2px;
           }
            #manageUsersSection p { /* Message when no users */
                 text-align: center;
                 font-style: italic;
                 color: #666;
            }
            #manageUsersSection .security-warning {
                color: #dc3545; /* Red */
                text-align: center;
                margin-top: 20px;
                font-weight: bold;
            }

            /* Styles for Professor Section */
             #professorSection h1 {
                 margin-bottom: 15px;
             }
             #professorSection p {
                 margin-bottom: 15px;
             }
              #professorSection label {
                  font-weight: bold;
                  margin-right: 5px;
              }
              #professorSection select {
                 margin-right: 15px;
              }
              #professorSection table.professor-table th, #professorSection table.professor-table td {
                   text-align: center; /* Center cells in professor table */
               }
               #professorSection table.professor-table td:first-child {
                  text-align: left; /* Student name aligned left */
               }
                /* Adjustment for observation cell in professor table */
               #professorSection table.professor-table td:last-child {
                   text-align: left; /* Align observations left */
               }

            /* Fixed footer in the application */
            #appFooter {
                width: 100%;
                height: 40px; /* Footer height */
                background-color: #00397a; /* SENAC Blue */
                color: white;
                text-align: center;
                line-height: 40px; /* Vertically center text */
                font-size: 0.9em;
                position: fixed; /* Fix to bottom */
                bottom: 0;
                left: 0;
                z-index: 1000;
            }

            /* Styles for Print Options in Student Bulletin Section */
            #studentPrintOptions {
                margin-bottom: 20px;
                padding: 15px;
                border: 1px solid #ddd;
                border-radius: 5px;
                background-color: #f9f9f9;
            }
            #studentPrintOptions label {
                display: inline-block;
                margin-right: 15px;
                font-weight: normal;
            }
            #studentPrintOptions input[type="checkbox"],
            #studentPrintOptions input[type="radio"] {
                margin-right: 5px;
                vertical-align: middle;
            }


        /* Print Styles (UPDATED) */
        @media print {
            body { margin: 0; display: block; background-color: #fff !important; color: #000; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; font-size: 10pt; }
            body:has(#loginContainer:not(.hidden)) { display: none !important; } /* Hide everything if on login screen */
             body { padding-bottom: 0 !important; } /* Remove body bottom padding on print */


            /* Hide UI elements */
            #sidebar, #loginContainer, .modal, .button, input, select,
             .unitCheckbox, .studentUnitCheckboxPrint, /* Hide all unit checkboxes */
             label[for="studentSelectPrint"], label[for="reportType"], label.admin-print-label, /* Hide admin print specific labels */
             #searchName, .close-button, th button, td button, #appHeaderText, .login-watermark,
             #loginContainer .senac-title, #manageUsersSection, #manageUsersSection table .button, #manageUsersSection .security-warning,
             #professorSection select, #professorSection label, #appFooter,
             #studentBulletinSection .button, /* Hide print button in student bulletin on print */
             #studentPrintOptions /* Hide the student's print options container */,
             #professorNotesSection .button, #professorNotesSection label, #professorNotesSection select, #professorNotesSection textarea,
             #professorNotesSection h3 /* Hide professor notes controls on print */
            { display: none !important; }

             #appContainer{ display: block; height: auto; width: 100%; overflow: visible; background-color: #fff !important;}
             #mainContentArea { width: 100%; padding: 0; overflow-y: visible; height: auto; flex-grow: 0; }

             /* Table Styles for Print */
             table { width: 100%; border-collapse: collapse; margin-top: 10px; }
             th, td { border: 1px solid #000; padding: 6px; text-align: center; font-size: 9pt; }
             th { background-color: #ddd !important; color: #000 !important; font-weight: bold; }
             td { text-align: center; } /* Ensure data cells are centered */
             td:first-child { text-align: left; } /* Align student name left in table */
             /* Adjustment for the new Observations column in print */
             #studentTable td:nth-last-child(2), #professorStudentTable td:nth-last-child(1) {
                 text-align: left; /* Align observation text left */
                 font-size: 8pt; /* Slightly smaller font if observation is long */
             }


             /* Control page breaks */
             tr { page-break-inside: avoid; page-break-after: auto; }
             table, tr, td, th { page-break-inside: avoid !important; } /* Prevent breaks inside table elements */
              .student-report { page-break-inside: avoid; page-break-after: always; margin-bottom: 0; padding: 10px 0;} /* Ensure report block stays together and force break after */
              .student-report:last-child { page-break-after: avoid; margin-bottom: 0; } /* Don't force break after the last report */

             /* Styles for each report header */
              .student-report div:first-child { text-align: center; margin-bottom: 10px; padding-bottom: 5px; border-bottom: 1px solid #ccc;}
              .student-report h2 { color: #000 !important; border-bottom: none; margin: 0; padding: 0; font-size: 14pt;}
              .student-report p { margin: 2px 0; font-size: 10pt;}
              .student-report strong { font-weight: bold; }

             /* Styles for the generated bulletin table */
             .bulletin-table {
                 width: 100%;
                 border-collapse: collapse;
                 margin-top: 15px;
             }
             .bulletin-table th, .bulletin-table td {
                 border: 1px solid #000;
                 padding: 8px;
                 text-align: center;
                 font-size: 9pt;
             }
             .bulletin-table th {
                 background-color: #eee !important; /* Light gray background for bulletin header */
                 color: #000 !important;
                 font-weight: bold;
             }
             .bulletin-table td {
                  text-align: center;
             }
             .bulletin-table td:first-child {
                 text-align: left; /* Discipline name aligned left */
             }
              .bulletin-table td:last-child {
                  text-align: left; /* Observation aligned left */
              }

             /* New styles for the bulletin header layout */
              .bulletin-header {
                  text-align: center;
                  margin-bottom: 20px; /* More space below the header block */
                  padding-bottom: 15px; /* More padding below the header content */
                  border-bottom: 2px solid #000; /* Thicker border */
              }
              .bulletin-header h2 {
                  margin-top: 0;
                  margin-bottom: 5px; /* Space below the main title */
                  font-size: 16pt; /* Slightly larger font */
                  color: #000; /* Ensure black color for printing */
              }
               .bulletin-header p {
                   margin: 5px 0; /* More space between paragraphs */
                   font-size: 11pt; /* Slightly larger font for student info */
                   color: #333; /* Ensure dark color */
               }
               .bulletin-header p strong {
                   font-weight: bold;
               }
                /* Specific styles for the consolidated header lines */
                .bulletin-header .line1 {
                    font-size: 18pt; /* Larger font for the main title line */
                    font-weight: bold;
                    margin-bottom: 8px; /* Space below line 1 */
                }
                 .bulletin-header .line2 {
                     font-size: 14pt; /* Slightly smaller for the secondary title */
                     margin-bottom: 8px; /* Space below line 2 */
                 }
                  .bulletin-header .line3 {
                      font-size: 11pt; /* Standard font for student info line */
                      margin-top: 8px; /* Add space above line 3 */
                      margin-bottom: 0; /* No margin below the last line of the header block */
                  }


             /* Page Margins */
              @page { size: A4; margin: 1.5cm; } /* Define page margins */
              body { padding: 0; } /* Remove body padding if margin is defined in @page */

              /* Ensure editable cell content is printed */
             .editable-cell span { display: inline !important; }

             /* New styles for professor notes section for print */
             .professor-notes-print {
                 margin-top: 25px;
                 padding-top: 15px;
                 border-top: 1px solid #ccc;
             }
             .professor-notes-print h4 {
                 font-size: 12pt;
                 margin-bottom: 10px;
                 color: #000;
             }
             .professor-note-item {
                 border: 1px solid #eee;
                 padding: 8px;
                 margin-bottom: 8px;
                 background-color: #f8f8f8;
             }
             .professor-note-item p {
                 margin: 0;
                 line-height: 1.4;
             }
             .professor-note-item strong {
                 font-weight: bold;
             }
             .professor-note-item span {
                 font-size: 9pt;
                 color: #666;
                 display: block;
                 margin-top: 5px;
             }


        }
         /* Styling for grouped student rows */
         .student-group-header td {
             background-color: #e9ecef; /* Light gray background for student header */
             font-weight: bold;
             text-align: left !important; /* Align student info left */
         }
          .student-group-header td:first-child {
              border-left: 3px solid #1a73e8; /* Blue left border */
          }

         .discipline-row td {
             border-top: none; /* Remove top border for discipline rows */
         }
         .discipline-row td:first-child {
             padding-left: 30px; /* Indent discipline name */
             text-align: left !important; /* Align discipline name left */
         }
          /* Adjust padding for other cells in discipline rows if needed */
          .discipline-row td:not(:first-child) {
              text-align: center; /* Center other cells */
          }
           .discipline-row td:last-child {
              text-align: center; /* Center actions cell */
           }

        /* Styles for Professor Notes Section */
        #professorNotesSection {
            margin-top: 30px;
            padding: 20px;
            border: 1px solid #ddd;
            border-radius: 8px;
            background-color: #fdfdfd;
        }
        #professorNotesSection h3 {
            border-bottom: 2px solid #1a73e8; /* Blue */
            margin-bottom: 15px;
        }
        #professorNotesSection .note-controls label {
            margin-right: 5px;
            font-weight: bold;
        }
        #professorNotesSection .note-controls select {
            margin-right: 15px;
        }
        #professorNotesSection textarea {
            width: 100%;
            min-height: 100px;
            padding: 10px;
            margin-top: 15px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box;
            font-size: 1em;
            resize: vertical;
        }
        #professorNotesSection .note-display {
            margin-top: 25px;
            border-top: 1px solid #eee;
            padding-top: 15px;
        }
        .professor-note-item {
            border: 1px solid #e0e0e0;
            background-color: #f9f9f9;
            padding: 12px;
            margin-bottom: 10px;
            border-radius: 6px;
            position: relative;
        }
        .professor-note-item p {
            margin: 0 0 5px 0;
            line-height: 1.4;
        }
        .professor-note-item strong {
            color: #1a73e8;
        }
        .professor-note-item span.note-meta {
            display: block;
            font-size: 0.85em;
            color: #777;
            margin-top: 8px;
            border-top: 1px solid #eee;
            padding-top: 5px;
        }
        .professor-note-item .delete-note-button {
            position: absolute;
            top: 10px;
            right: 10px;
            background-color: #dc3545;
            color: white;
            border: none;
            border-radius: 4px;
            padding: 4px 8px;
            font-size: 0.75em;
            cursor: pointer;
            transition: background-color 0.2s ease;
        }
        .professor-note-item .delete-note-button:hover {
            background-color: #c82333;
        }
        #studentProfessorNotes {
            margin-top: 30px;
            padding: 20px;
            border: 1px solid #d4edda; /* Light green border for student notes */
            border-radius: 8px;
            background-color: #e9f7ef; /* Light green background */
        }
        #studentProfessorNotes h2 {
            border-bottom: 2px solid #28a745; /* Green */
            color: #218838; /* Darker green */
            margin-bottom: 20px;
        }
        #studentProfessorNotes .no-notes {
            text-align: center;
            font-style: italic;
            color: #555;
        }
</style>
</head>
<body>
<div id="loginContainer">
<div class="login-watermark">
<img alt="Logo SENAC" src="senac_logo.png"/>
</div>
<h1 class="senac-title">SENAC</h1>
<p style="color: #ff6600; margin-top: -20px; margin-bottom: 20px; font-size: 1.2em;">Boletim Escolar - MÉDIOTEC</p> <input id="username" placeholder="Usuário" type="text"/>
<input id="password" placeholder="Senha" type="password"/>
<button class="button" id="loginButton" type="button">Entrar</button>
<p class="error" id="loginError"></p>
</div>
<div class="hidden" id="appContainer">
<div id="sidebar">
<button class="button" onclick="goHome()" type="button">Voltar ao Início</button>
<div class="hidden" id="userManagementSection">
<h3>Gerenciar Usuários</h3>
<button class="button" onclick="showAddProfessorForm()" type="button">Adicionar Professor</button>
<button class="button" onclick="showAddCoordenadorForm()" type="button">Adicionar Coordenador</button>
<button class="button" onclick="showManageUsersSection()" type="button">Gerenciar Professores/Coordenadores</button><button class="button" onclick="showAddAlunoUserForm()" type="button">Adicionar Usuário de Aluno</button>
</div>
<button class="button hidden" id="professorAgendaButton" onclick="showProfessorNotesSection()" type="button">Agenda do Professor</button>
<hr class="hidden" id="sidebarHr"/>
<button class="button logout-button" onclick="logout()" type="button">Sair</button>
</div>
<div id="mainContentArea">
<div id="appHeaderText">MÉDIOTEC</div>
<div class="hidden" id="studentBulletinSection">
    <h1>Meu Boletim</h1>
    <div id="studentPrintOptions">
        <h3>Opções de Impressão:</h3>
        <div>
            <label class="admin-print-label">Selecione as Unidades:</label>
            <div>
                <label><input class="studentUnitCheckboxPrint" type="checkbox" value="1" checked/> Unidade 1</label>
                <label><input class="studentUnitCheckboxPrint" type="checkbox" value="2" checked/> Unidade 2</label>
                <label><input class="studentUnitCheckboxPrint" type="checkbox" value="3" checked/> Unidade 3</label>
            </div>
        </div>
        <div style="margin-top: 10px;">
            <label class="admin-print-label"><input name="studentReportType" type="radio" value="full" checked/> Incluir Avaliações</label>
            <label class="admin-print-label"><input name="studentReportType" type="radio" value="summary"/> Somente Menção e Situação</label>
        </div>
    </div>
    <button class="button" onclick="printMyBulletin()" type="button" style="width: auto; margin-bottom: 20px;">Imprimir Meu Boletim</button>
    <div id="studentBulletinContent">
        <p style="text-align: center; font-style: italic;">Carregando boletim...</p>
    </div>
    <div id="studentProfessorNotes">
        <h2>Anotações dos Professores</h2>
        <div id="studentNotesContent">
            <p class="no-notes">Nenhuma anotação disponível.</p>
        </div>
    </div>
</div>

<div class="hidden" id="studentManagementSection"> <h1>Gerenciador de Notas de Boletim</h1>
<div class="hidden" id="addStudentSection"> <h2>Adicionar Aluno</h2>
<input id="studentName" placeholder="Nome do Aluno" type="text"/>
<select id="class">
<option value="">Selecione a Turma</option>
<option value="1A">1A</option> <option value="1B">1B</option>
<option value="1C">1C</option>
<option value="2A">2A</option>
<option value="2B">2B</option>
<option value="3A">3A</option>
</select>
<button class="button" onclick="addStudent()" type="button">Adicionar Aluno</button>
</div>
<div class="hidden" id="addDisciplineSection"> <h3>Adicionar Disciplina ao Aluno</h3>
<div>
<label for="disciplineClassSelect">Selecione a Turma:</label>
<select id="disciplineClassSelect">
<option value="">Selecione a Turma</option>
</select>
</div>
<div style="margin-top: 10px;">
<label for="studentSelect">Selecione o Aluno:</label>
<select disabled="" id="studentSelect"> <option value="">Selecione a Turma Primeiro</option>
</select>
</div>
<div style="margin-top: 10px;">
<label for="disciplineSelect">Selecione a Disciplina:</label>
<select id="disciplineSelect">
<option value="">Selecione a Disciplina</option>
<option value="Redação">Redação</option>
<option value="Gramática">Gramática</option>
<option value="Educação Física">Educação Física</option>
<option value="Literatura">Literatura</option>
<option value="Geografia">Geografia</option>
<option value="Inglês">Inglês</option>
<option value="História">História</option>
<option value="Projeto de Vida">Projeto de Vida</option>
<option value="Artes">Artes</option>
<option value="Matemática">Matemática</option>
<option value="Filosofia">Filosofia</option>
<option value="Física">Física</option>
<option value="Química">Química</option>
<option value="Biologia">Biologia</option>
<option value="Formação Profissional">Formação Profissional</option>
<option value="Inovaê">Inovaê</option>
<option value="Sociologia">Sociologia</option>
</select>
</div>
<div style="margin-top: 10px;">
<label for="unitSelect">Selecione a Unidade:</label>
<select id="unitSelect">
<option value="">Selecione a Unidade</option>
<option value="1">1° Unidade</option>
<option value="2">2° Unidade</option>
<option value="3">3° Unidade</option>
</select>
</div>
<div style="margin-top: 10px;">
<label for="evaluation1">Avaliação 1:</label>
<select id="evaluation1">
<option value="">Avaliação 1</option>
<option value="A">A</option>
<option value="PA">PA</option>
<option value="ND">ND</option>
</select>
</div>
<div style="margin-top: 10px;">
<label for="evaluation2">Avaliação 2:</label>
<select id="evaluation2">
<option value="">Avaliação 2</option>
<option value="A">A</option>
<option value="PA">PA</option>
<option value="ND">ND</option>
</select>
</div>
<div style="margin-top: 10px;">
<label for="finalGrade">Menção Final:</label>
<select id="finalGrade">
<option value="">Menção Final</option>
<option value="D">Desenvolveu (D)</option>
<option value="ND">Não Desenvolveu (ND)</option>
</select>
</div>
<div style="margin-top: 10px;">
<label for="observationInput">Observações (Opcional):</label>
<input id="observationInput" placeholder="Observações (Opcional)" type="text"/>
</div>
<button class="button" onclick="addDiscipline()" style="margin-top: 15px;" type="button">Adicionar Disciplina</button>
</div>
<h2>Consultar Alunos</h2>
<input id="searchName" placeholder="Pesquisar Aluno" type="text"/>
<button class="button" onclick="searchStudent()" type="button">Pesquisar</button>

<h3>Imprimir Boletim</h3>
<div id="printControls"> <div>
        <label for="printClassSelect" class="admin-print-label">Selecione a Turma:</label>
        <select id="printClassSelect">
            <option value="">Selecione a Turma</option>
        </select>
    </div>
    <div style="margin-top: 10px;">
        <label for="studentSelectPrint" class="admin-print-label">Selecione o Aluno (Opcional para impressão da turma):</label>
        <select disabled="" id="studentSelectPrint"> <option value="">Selecione a Turma Primeiro</option>
        </select>
    </div>
    <div style="margin-top: 10px;">
        <label class="admin-print-label">Selecione as Unidades:</label>
        <div>
            <label><input class="unitCheckbox" type="checkbox" value="1" checked/> Unidade 1</label>
            <label><input class="unitCheckbox" type="checkbox" value="2" checked/> Unidade 2</label>
            <label><input class="unitCheckbox" type="checkbox" value="3" checked/> Unidade 3</label>
        </div>
    </div>
    <div style="margin-top: 10px;">
        <label class="admin-print-label"><input name="reportType" type="radio" value="full" checked/> Incluir Avaliações</label>
        <label class="admin-print-label"><input name="reportType" type="radio" value="summary"/> Somente Menção e Situação</label>
    </div>
    <button class="button" onclick="printStudentReport()" type="button">Imprimir Boletim do Aluno</button>
    <button class="button" onclick="printClassReports()" type="button">Imprimir Boletins da Turma</button>
    <button class="button" onclick="printAllReports()" type="button">Imprimir Todos os Boletins (Todos Alunos)</button>
</div>


<h3>Backup/Restaurar Dados (Manual)</h3>
<button class="button" onclick="exportData()" type="button">Exportar Dados (Backup)</button>
<div style="margin-top: 10px; border: 1px solid #ccc; padding: 10px; border-radius: 5px;">
<p style="margin-top: 0; font-weight: bold;">Importar Dados:</p>
<input accept=".json" id="importFile" style="display:none;" type="file"/>
<button class="button" onclick="document.getElementById('importFile').click()" style="width: auto; display: inline-block; margin: 5px 0;" type="button">Selecionar Arquivo</button>
<span id="importFileName" style="margin-left: 10px; font-style: italic;">Nenhum arquivo selecionado.</span>
<button class="button delete-button" disabled="" id="performImportButton" onclick="importData()" style="width: auto; display: inline-block; margin: 5px;" type="button">Confirmar Importação (Irá substituir dados atuais!)</button>
<p class="error" id="importStatus" style="margin-top: 5px;"></p>
</div>
<table id="studentTable">
<thead>
<tr>
<th>Nome</th>
<th>Curso</th>
<th>Turma</th>
<th>Disciplina</th>
<th>Unidade</th>
<th>Avaliação 1</th>
<th>Avaliação 2</th>
<th>Menção Final</th>
<th>Situação</th>
<th>Observações</th> <th>Ações</th>
</tr>
</thead>
<tbody>
</tbody>
</table>
</div>
<div class="hidden" id="manageUsersSection"> <h1>Gerenciar Professores e Coordenadores</h1>
<button class="button" onclick="showStudentManagementSection()" style="width:auto; margin-bottom: 20px;" type="button">Voltar para Gerenciar Alunos</button>
<p class="security-warning">AVISO DE SEGURANÇA: As senhas estão visíveis nesta tela apenas para demonstração. Em um sistema real, senhas nunca devem ser exibidas ou armazenadas em texto plano.</p>
<p id="noUsersMessage">Nenhum usuário (Professor ou Coordenador) cadastrado além do administrador principal.</p>
<table id="usersTable">
<thead>
<tr>
<th>Nome</th>
<th>Usuário</th>
<th>Papel</th>
<th>Disciplinas Atribuídas</th>
<th>Turmas Atribuídas</th>
<th>Senha</th>
<th>Ações</th>
</tr>
</thead>
<tbody>
</tbody>
</table>
</div>
<div class="hidden" id="professorSection"> <h1 id="professorWelcome">Olá, Professor(a)!</h1> <p>Selecione a disciplina, turma e unidade para lançar ou editar as notas dos alunos.</p>
<div>
<label for="professorDisciplineSelect">Disciplina:</label>
<select id="professorDisciplineSelect">
<option value="">Selecione a Disciplina</option>
</select>
<label for="professorClassSelect" style="margin-left: 20px;">Turma:</label>
<select id="professorClassSelect">
<option value="">Selecione a Turma</option>
</select>
<label for="professorUnitSelect" style="margin-left: 20px;">Unidade:</label>
<select id="professorUnitSelect">
<option value="">Selecione a Unidade</option>
<option value="1">1° Unidade</option>
<option value="2">2° Unidade</option>
<option value="3">3° Unidade</option>
</select>
</div>
<table class="professor-table" id="professorStudentTable" style="margin-top: 20px;">
<thead>
<tr>
<th>Nome do Aluno</th>
<th>Curso</th>
<th>Turno</th>
<th>Avaliação 1</th>
<th>Avaliação 2</th>
<th>Menção Final</th>
<th>Situação</th>
<th>Observações</th> </tr>
</thead>
<tbody>
</tbody>
</table>

<p id="professorNoStudentsMessage" style="text-align: center; font-style: italic; margin-top: 20px;">Selecione a disciplina, turma e unidade acima para ver os alunos.</p>
</div>

<div class="hidden" id="professorNotesSection">
    <h1>Agenda de Anotações do Professor</h1>
    <p>Adicione anotações para os alunos por disciplina e unidade. Estas anotações ficarão visíveis no boletim do aluno.</p>

    <div class="note-controls" style="margin-bottom: 20px;">
        <label for="notesDisciplineSelect">Disciplina:</label>
        <select id="notesDisciplineSelect">
            <option value="">Selecione a Disciplina</option>
            </select>

        <label for="notesClassSelect">Turma:</label>
        <select id="notesClassSelect">
            <option value="">Selecione a Turma</option>
            </select>

        <label for="notesStudentSelect">Aluno:</label>
        <select id="notesStudentSelect">
            <option value="">Selecione o Aluno</option>
            </select>

        <label for="notesUnitSelect">Unidade:</label>
        <select id="notesUnitSelect">
            <option value="">Selecione a Unidade</option>
            <option value="1">1° Unidade</option>
            <option value="2">2° Unidade</option>
            <option value="3">3° Unidade</option>
        </select>
    </div>

    <h3>Nova Anotação</h3>
    <textarea id="professorNoteContent" placeholder="Escreva sua anotação aqui..."></textarea>
    <button class="button" onclick="saveProfessorNote()" style="width: auto; margin-top: 10px;" type="button">Salvar Anotação</button>

    <div class="note-display">
        <h3>Anotações Existentes</h3>
        <div id="existingProfessorNotes">
            <p class="no-notes" style="text-align: center; font-style: italic;">Nenhuma anotação encontrada para esta seleção.</p>
        </div>
    </div>
</div>

</div>
<footer id="appFooter">SENAC</footer>
</div>
<div class="modal hidden" id="addProfessorModal">
<div class="modal-content">
<span class="close-button" onclick="closeAddProfessorModal()">×</span>
<h2>Adicionar Novo Professor</h2>
<div>
<label for="newProfessorNameInput">Nome Completo:</label>
<input id="newProfessorNameInput" placeholder="Nome completo do professor" type="text"/>
</div>
<div>
<label for="newProfessorUsernameInput">Usuário (Login):</label>
<input id="newProfessorUsernameInput" placeholder="Nome de usuário para login" type="text"/>
</div>
<div>
<label for="newProfessorPasswordInput">Senha:</label>
<input id="newProfessorPasswordInput" placeholder="Senha para login" type="password"/>
</div>
<h3>Atribuições por Disciplina e Turma:</h3>
<div id="newProfessorAssignments">
    </div>
<button class="button" onclick="saveProfessor()" style="width: auto; display: inline-block; margin-top:20px;" type="button">Salvar Professor</button>
</div>
</div>
<div class="modal hidden" id="addCoordinatorModal">
<div class="modal-content">
<span class="close-button" onclick="closeAddCoordenadorModal()">×</span>
<h2>Adicionar Novo Coordenador</h2>
<div>
<label for="newCoordinatorNameInput">Nome Completo:</label>
<input id="newCoordinatorNameInput" placeholder="Nome completo do coordenador" type="text"/>
</div>
<div>
<label for="newCoordinatorUsernameInput">Usuário (Login):</label>
<input id="newCoordinatorUsernameInput" placeholder="Nome de usuário para login" type="text"/>
</div>
<div>
<label for="newCoordinatorPasswordInput">Senha:</label>
<input id="newCoordinatorPasswordInput" placeholder="Senha para login" type="password"/>
</div>
<p style="margin-top: 15px; color: #555;">Coordenadores têm acesso total ao sistema, exceto a criação/edição de usuários.</p>
<button class="button" onclick="saveCoordinator()" style="width: auto; display: inline-block; margin-top:20px;" type="button">Salvar Coordenador</button>
</div>
</div>
<div class="modal hidden" id="editUserModal">
<div class="modal-content">
<span class="close-button" onclick="closeEditUserModal()">×</span>
<h2 id="editUserModalTitle">Editar Usuário</h2>
<input id="editUserOriginalUsername" type="hidden"/>
<input id="editUserRole" type="hidden"/>
<div>
<label for="editUserNameInput">Nome Completo:</label>
<input id="editUserNameInput" placeholder="Nome completo" type="text"/>
</div>
<div>
<label for="editUserUsernameInput">Usuário (Login):</label>
<input id="editUserUsernameInput" placeholder="Nome de usuário para login" type="text"/>
<small style="display: block; margin-top: 5px; color: #555;">Alterar o usuário pode exigir um novo login.</small>
</div>
<div>
<label for="editUserPasswordInput">Senha:</label>
<input id="editUserPasswordInput" placeholder="Deixe em branco para manter a senha atual" type="password"/>
<small style="display: block; margin-top: 5px; color: #555;">Deixe este campo em branco para não alterar a senha.</small>
</div>
<div id="editProfessorAssignments">
<h3>Atribuições por Disciplina e Turma:</h3>
    </div>
<button class="button" onclick="updateUser()" style="width: auto; display: inline-block; margin-top:20px;" type="button">Atualizar Usuário</button>
</div>
</div>
<script>
    // --- DATA SIMULATION IN MEMORY (Replace with Local Storage or Backend) ---
    // Initial data (will be replaced by localStorage if data exists)
     let students = [];
     let users = [{ username: 'administrador', password: 'admsenac2024', role: 'admin', name: 'Administrador', assignments: [] }]; // Default admin user
    const newStudentsList1A = ["Alice Souza Cavalcanti", "Ana Luisa Trajano Fragoso", "Anna Jullia Cabral da Silva", "Davi Fenelon Mendonça da Silva", "Fernando Vinicius Serejo de Melo", "Gabriel Artur Lima da Silva", "Gabriel de Souza Alencar", "Gabriel Vinicius Nazareth de Melo", "Gabrielly Maria da Silva Oliveira", "Giovanna Dias Sales do Nascimento", "Guilherme Barbosa Alcântara", "Guilherme Viana Guedes Nunes", "Heloisa Maria do Carmo Santos", "Henrique Rodrigues Ulisses", "Hivison Yan Pereira de Oliveira", "Ikaro Vinícius Gomes de Abreu", "Isabel Cristina Vital da Silva Nascimento", "Julia Carlos Picchetto", "Leonardo Vasconcelos Santana", "Leticia Xavier da Silva", "Marcos Cavalcanti de Freitas Lima", "Maria Laura Alves da Silva", "Maria Luiza Gomes Felix", "Matheus Josias de França Caitano", "Miguel Bispo de Almeida", "Miguel Rodrigues Souza de Lima", "Milena Maria Andrade de Lima", "Nicolas Gabriel de Oliveira Muniz", "Otávio Xavier de Amorim Fontes", "Pedro Luiz Barbosa Generoso", "Rayner Victor Silva de Rezende Filho", "Richard Lima Nascimento de Melo", "Yasmin de França Medeiros"];
    const newStudentsList1B = ['Álvaro Martins Moraes', 'Anderson Marcelino dos Santos', 'Anna Luiza Vicente de Castro Lira', 'Arthur Brunno dos Santos Silva', 'Arthur Guilherme de Andrade Barros', 'Caylane Maria Rodrigues de Souza', 'Clara Beatriz Viana Souza Pinto', 'Davi Cruz Correia Lima', 'Eduardo Bezerra Souto Maior Mendes', 'Ellen Vitória Pessoa da Silva', 'Gabriel Araújo de França', 'Guilherme Vinhaes de Matos', 'Isabelle Miranda Moreira', 'Israel Ricardo de Araujo Lõbo', 'Izadhora Luíza Dias Pródigo', 'Jorge Henrique da Silva', 'Julia Isabella Bezerra de Fraga', 'Júlia Letícia do Nascimento da Silva', 'Karen Lorena da Cunha Souza', 'Lucas da Silva Resende', 'Marcelo José Bomfim Neto', 'Marcos Yuri Gadelha Araújo', 'Maria Clara Bezerra Reis', 'Maria Eduarda Araújo Nascimento', 'Maria Eduarda Rodrigues Barreto', 'Maria Letícia Góes Azevedo', 'Marina Gabriele de Andrade Rego', 'Matheus Dias Correia de Assunção', 'Murilo de Lima Rodrigues', 'Pedro Gabriel Farias de Arruda Guedes', 'Ruan Zaquel Sena da Silva', 'Samara Maciel Cabral de Melo'];
    const newStudentsList1C = ['Alice Giulianna dos Santos Alves', 'Arthur Brito Solano Guerra de Oliveira', 'Caua Leonardo Santos Ferreira de Sena', 'Cláudio Gusmão Ramos Neto', 'David Romão Gomes dos Passos', 'Emily Larissa Pereira da Silva', 'Estefany Guedes Carvalho', 'Evilin Nayara Gomes da Silva', 'Harison Cleyton Fernandes do Nascimento', 'Heitor Assunção Monteiro', 'Izaias Elias Chagas Neto', 'Jefferson Luan Silva de Paula', 'João Daniel Bernardo de Santana Oliveira', 'João Pedro da SilvaEm Processo', 'Jullia Hadassa Araujo de Oliveira', 'Kaike Eduardo Morozini de Bastos', 'Kauã Felipe Oliveira da Silva', 'Larissa Lopes Belo da Silva', 'Livia Ferreira Nunes', 'Luís Guilherme Ventura Alves', 'Maria Clara da Silva Clemente', 'Maria Eduarda de Brito Rodrigues', 'Maria Eduarda Farias Chaves Lopes', 'Maria Fernanda Oliveira Costa', 'Maria Klara Alves Cavalcante', 'Maria Luiza Braz Mendes', 'Mateus Marinho Espíndola', 'Nicolas Gabriel Bezerra dos Santos Souza', 'Rianne de Almeida Romao', 'Samuell Moises Pereira da Silva', 'Thays Camila da Silva Santos', 'Vinicios Bezerra da Silva', 'Wandersson Alves Cavalcante', 'William Alves de Freitas'];
    const newStudentsList2A = ['Cibele Guerra Medeiros', 'Davi Nascimento Martins', 'Deborah Leão Marques Machado', 'Erick Cauã Ferreira Rodrigues Figueredo', 'Gabriel Elias Rangel', 'Gleiciane Júlia Vieira do Nascimento', 'Ivan Freire de Araújo Neto', 'Joao Henrique Oliveira Gonçalves', 'Joao Vitor Sousa Ramos', 'Juan Gomes Rodrigues', 'Leticia de Santana Lins', 'Letícia Maria Augusto de Souza Galvão', 'Luckas Alexandre Oliveira Castro Barros', 'Luna Ariela Carvalho de Deus', 'Matheus de Andrade Cordeiro Malafaia Gomes', 'Matheus Henrique Ferreira da Silva', 'Maxwell Bernardo Eulálio Pereira Cavalcante', 'Pedro Vinicius de Souza Cavalcanti', 'Pietro Cauê da Silva Santos', 'Renato Alves Ribeiro de Oliveira', 'Suzanny Moura de Barros', 'Talles Renan Melo de Souza Lira', 'Thayná Valença Albuquerque', 'Victor Gabriel Pereira de Lima', 'Vinicius Novaes Silva'];
    const newStudentsList2B = ['Adricia Naine Costa Bandeira Ferreira', 'Airton Samuel Rodrigues Costa', 'Bruno Rafael Silva Costa', 'Caio Cesar Silva Melo', 'Caio Muller Silva da Rocha', 'Daniel Henrique José dos Santos', 'Davi Felix MarinhoEm', 'Diógenes Luiz Freitas Batista', 'Eduardo Passos de Andrade', 'Gabriela Lima Alexandrino', 'Geovana Noemi Ferreira Moura', 'Guilherme José Rodrigues de Freitas', 'Ingrid Cristina Rodrigues Ventura de Araújo', 'João Henrique Santana Cunha', 'Layza Cristina Melo Santos', 'Ligia Vitoria Linhares do Nascimento', 'Lucas Miguel Barbosa da Silva', 'Luiz Henrique dos Santos', 'Marcos Vasconcelos Dencker Beltrão', 'Maria Carolina Franco de Lima Leite', 'Maria Manuela Helena da Silva', 'Maria Manuela Helena da Silva', 'Matheus Vínicius Aguiar da Silva', 'Pedro Henrique Moura Nascimento da Silva', 'Pedro Henrique Ribeiro de Assumpção', 'Pedro Ivo Ribeiro da Cunha', 'Pedro Vinicius Barbosa de Oliveira', 'Sidney Sabino de Lima Junior'];
    const newStudentsList3A = ['Alliny Santos de Albuquerque', 'Anna Bianca de Moraes Almeida Castro', 'Arthur Emmanuel França Aquino de Medeiros', 'Carlos Eduardo Bezerra de Santana', 'Clarice Araújo Soares Couto', 'Emily Cristiane Mesquita de Almeida', 'Filipe Emanuel Lins do Nascimento Silva', 'Guilherme Tomaz Paiva de Aquino', 'Iury Gabriel Barreto', 'Lara Lins de Oliveira', 'Leandro Henrique Carvalho Correia', 'Leticia Chaprão Aureliano de Souza', 'Leticia Ellen Ximenes', 'Letícia Marques de Lima Campos', 'Lucas Bandeira de Melo Torres Sena', 'Marcello Henrique da Silva Barros', 'Maria Clara Oliveira Sarinho de Melo', 'Maria Eduarda Soares de Albuquerque', 'Maria Livia Costa de Oliveira Silva', 'Marina Rodrigues de Lima', 'Mateus Araujo de Oliveira Santos', 'Samara Marinho Pereira Roberto', 'Victor Antonio Santana de Lima', 'Yasmin Lopes Mendes'];
    const classInfoMap = { '1A': { unit: 'Manhã', course: 'Médio Técnico Desenvolvimento de Sistemas' }, '1B': { unit: 'Manhã', course: 'Médio Técnico Desenvolvimento de Jogos' }, '1C': { unit: 'Tarde', course: 'Médio Técnico Desenvolvimento de Sistemas' }, '2A': { unit: 'Manhã', course: 'Médio Técnico Desenvolvimento de Sistemas' }, '2B': { unit: 'Manhã', course: 'Médio Técnico Desenvolvimento de Sistemas' }, '3A': { unit: 'Tarde', course: 'Médio Técnico Informática' } };
    function getClassInfo(className) { return classInfoMap[className] || { unit: 'Desconhecido', course: 'Desconhecido' }; }

    // --- Local Storage Functions ---
    function saveData() {
        try {
            localStorage.setItem('students', JSON.stringify(students));
            localStorage.setItem('users', JSON.stringify(users));
             // console.log('Data saved to localStorage');
        } catch (e) {
            console.error('Error saving data to localStorage:', e);
            alert('Não foi possível salvar os dados no navegador. Por favor, verifique se o armazenamento local está ativado.');
        }
    }

    function loadData() {
        try {
            const savedStudents = localStorage.getItem('students');
            const savedUsers = localStorage.getItem('users');
            if (savedStudents) {
                students = JSON.parse(savedStudents);
                // Ensure students have a unique ID if they don't already (for backward compatibility)
                if (students.length > 0) {
                     students = students.map((s, index) => {
                         if (!s.id) s.id = 's' + (index + 1);
                         if (!s.professorNotes) s.professorNotes = []; // NEW: Ensure professorNotes array exists
                         return s;
                     });
                }
                 // console.log('Students data loaded from localStorage');
            } else {
                 // Populate with initial student data if no data in localStorage
                 populateInitialStudents();
                 // console.log('No student data found in localStorage, using initial data.');
            }

            if (savedUsers) {
                users = JSON.parse(savedUsers);
                 // Ensure the default admin user exists if loading from a state where it might be missing
                 if (!users.find(user => user.username === 'administrador')) {
                     users.unshift({ username: 'administrador', password: 'admsenac2024', role: 'admin', name: 'Administrador', assignments: [] });
                 }
                 // NEW: Ensure all users have an assignments array if missing
                 users = users.map(user => {
                     if (!Array.isArray(user.assignments)) user.assignments = [];
                     return user;
                 });
                 // console.log('Users data loaded from localStorage');
            } else {
                 // Add initial professor and coordinator data if no user data in localStorage
                 users = [{ username: 'administrador', password: 'admsenac2024', role: 'admin', name: 'Administrador', assignments: [] }];
                 users.push({ username: 'profmath', password: 'passprof', role: 'professor', name: 'Prof. Matemática', assignments: [{ discipline: 'Matemática', classes: ['1A', '1B', '1C', '2A', '2B', '3A'] }, { discipline: 'Física', classes: ['2A', '2B'] }] });
                 users.push({ username: 'coord', password: 'passcoord', role: 'coordenador', name: 'Coordenador Geral', assignments: [] });
                  // console.log('No user data found in localStorage, using initial data.');
            }
        } catch (e) {
            console.error('Error loading data from localStorage:', e);
            alert('Não foi possível carregar os dados do navegador. Os dados iniciais serão usados.');
             // Fallback to initial data if loading fails
            students = [];
            users = [{ username: 'administrador', password: 'admsenac2024', role: 'admin', name: 'Administrador', assignments: [] }];
             populateInitialStudents();
             users.push({ username: 'profmath', password: 'passprof', role: 'professor', name: 'Prof. Matemática', assignments: [{ discipline: 'Matemática', classes: ['1A', '1B', '1C', '2A', '2B', '3A'] }, { discipline: 'Física', classes: ['2A', '2B'] }] });
             users.push({ username: 'coord', password: 'passcoord', role: 'coordenador', name: 'Coordenador Geral', assignments: [] });
        }
    }

     // Helper to populate initial student data (only used if no data in localStorage)
     function populateInitialStudents() {
        students = []; // Clear existing students if any
         const studentLists = { '1A': newStudentsList1A, '1B': newStudentsList1B, '1C': newStudentsList1C, '2A': newStudentsList2A, '2B': newStudentsList2B, '3A': newStudentsList3A };
         Object.keys(studentLists).forEach(className => {
             const classInfo = getClassInfo(className);
             studentLists[className].forEach(studentName => {
                 students.push({ id: 's' + (students.length + 1), name: studentName, course: classInfo.course, class: className, unit: classInfo.unit, disciplines: [], professorNotes: [] }); // NEW: Initialize professorNotes
             });
         });
     }


    let currentUser = null;
    const loginContainer = document.getElementById('loginContainer');
    const appContainer = document.getElementById('appContainer');
    const studentManagementSection = document.getElementById('studentManagementSection');
    const manageUsersSection = document.getElementById('manageUsersSection');
    const professorSection = document.getElementById('professorSection');
    const professorNotesSection = document.getElementById('professorNotesSection'); // NEW
    const userManagementSection = document.getElementById('userManagementSection');
    const sidebarHr = document.getElementById('sidebarHr');
    const professorWelcome = document.getElementById('professorWelcome');
    const studentBulletinSection = document.getElementById('studentBulletinSection');
    const studentPrintOptions = document.getElementById('studentPrintOptions'); // Container for student's print options
    const professorAgendaButton = document.getElementById('professorAgendaButton'); // NEW


    function showSection(sectionToShow) {
        studentManagementSection.classList.add('hidden');
        manageUsersSection.classList.add('hidden');
        professorSection.classList.add('hidden');
        studentBulletinSection.classList.add('hidden');
        professorNotesSection.classList.add('hidden'); // NEW
        sectionToShow.classList.remove('hidden');
    }

    function showStudentManagementSection() {
        showSection(studentManagementSection);
        renderStudentTable(students);
        populateClassSelect(document.getElementById('printClassSelect'));
        populateStudentPrintSelect();
        if (currentUser && (currentUser.role === 'admin' || currentUser.role === 'coordenador')) {
            document.getElementById('addStudentSection').classList.remove('hidden');
            document.getElementById('addDisciplineSection').classList.remove('hidden');
            populateClassSelect(document.getElementById('disciplineClassSelect'));
            populateStudentSelectForDiscipline();
            professorAgendaButton.classList.add('hidden'); // NEW: Hide agenda button for admin/coordinator
        } else {
            document.getElementById('addStudentSection').classList.add('hidden');
            document.getElementById('addDisciplineSection').classList.add('hidden');
        }
    }

    function showManageUsersSection() {
        showSection(manageUsersSection);
        renderUsersTable(users);
        professorAgendaButton.classList.add('hidden'); // NEW
    }

    function showProfessorSection(user) {
        showSection(professorSection);
        professorWelcome.textContent = 'Olá, ' + user.name + '!';
        populateProfessorSelects(user);
        document.querySelector('#professorStudentTable tbody').innerHTML = '';
        document.getElementById('professorNoStudentsMessage').classList.remove('hidden');
         document.querySelector('#professorStudentTable thead').classList.add('hidden'); // Hide table header initially
        professorAgendaButton.classList.remove('hidden'); // NEW: Show agenda button for professor
    }

    // NEW: Function to show Professor Notes Section
    function showProfessorNotesSection() {
        showSection(professorNotesSection);
        professorAgendaButton.classList.remove('hidden'); // Keep agenda button visible
        populateProfessorNotesControls();
        document.getElementById('professorNoteContent').value = ''; // Clear textarea
        document.getElementById('existingProfessorNotes').innerHTML = '<p class="no-notes" style="text-align: center; font-style: italic;">Selecione aluno, disciplina e unidade para ver ou adicionar anotações.</p>'; // Clear existing notes
    }


    function showStudentBulletin() {
        showSection(studentBulletinSection);
        if (currentUser && currentUser.role === 'aluno') {
            const student = students.find(s => s.id === currentUser.studentId);
            const bulletinContentDiv = document.getElementById('studentBulletinContent');
            if (student) {
                // Load all units by default for initial display
                const allUnits = [...new Set(student.disciplines.map(d => d.unit))].sort((a, b) => a - b);
                const bulletinHTML = generateBulletinHTML(student, allUnits, 'full'); // Display full, all units
                bulletinContentDiv.innerHTML = bulletinHTML;

                // Make print options visible
                if(studentPrintOptions) studentPrintOptions.classList.remove('hidden');
                // Check all unit checkboxes by default for the student
                document.querySelectorAll('.studentUnitCheckboxPrint').forEach(cb => cb.checked = true);
                // Set default report type for student
                const studentReportTypeFull = document.querySelector('input[name="studentReportType"][value="full"]');
                if(studentReportTypeFull) studentReportTypeFull.checked = true;

                displayStudentNotes(student); // NEW: Display professor notes for the student

            } else {
                bulletinContentDiv.innerHTML = '<p style="text-align: center; font-style: italic;">Não foi possível encontrar seus dados de boletim.</p>';
                if(studentPrintOptions) studentPrintOptions.classList.add('hidden');
                document.getElementById('studentNotesContent').innerHTML = '<p class="no-notes">Nenhuma anotação disponível.</p>'; // NEW: Clear student notes
            }
        } else {
            document.getElementById('studentBulletinContent').innerHTML = '<p style="text-align: center; font-style: italic;">Você precisa estar logado como aluno para ver seu boletim.</p>';
             if(studentPrintOptions) studentPrintOptions.classList.add('hidden');
             document.getElementById('studentNotesContent').innerHTML = '<p class="no-notes">Nenhuma anotação disponível.</p>'; // NEW: Clear student notes
        }
        userManagementSection.classList.add('hidden');
        sidebarHr.classList.add('hidden');
        professorAgendaButton.classList.add('hidden'); // NEW
    }


    function goHome() {
        if (currentUser) {
            if (currentUser.role === 'admin' || currentUser.role === 'coordenador') {
                showStudentManagementSection();
                userManagementSection.classList.remove('hidden');
                sidebarHr.classList.remove('hidden');
                professorAgendaButton.classList.add('hidden'); // NEW
            } else if (currentUser.role === 'professor') {
                showProfessorSection(currentUser);
                userManagementSection.classList.add('hidden');
                sidebarHr.classList.add('hidden');
                professorAgendaButton.classList.remove('hidden'); // NEW
            } else if (currentUser.role === 'aluno') {
                showStudentBulletin();
                userManagementSection.classList.add('hidden');
                sidebarHr.classList.add('hidden');
                professorAgendaButton.classList.add('hidden'); // NEW
            }
        } else {
            logout();
        }
    }


    function logout() {
        currentUser = null;
        localStorage.removeItem('currentUser');
        appContainer.classList.add('hidden');
        loginContainer.classList.remove('hidden');
        document.getElementById('username').value = '';
        document.getElementById('password').value = '';
        document.getElementById('loginError').textContent = '';
        userManagementSection.classList.add('hidden');
        sidebarHr.classList.add('hidden');
        professorAgendaButton.classList.add('hidden'); // NEW
        document.querySelector('#studentTable tbody').innerHTML = '';
        document.querySelector('#usersTable tbody').innerHTML = '';
        document.querySelector('#professorStudentTable tbody').innerHTML = '';
        document.getElementById('addStudentSection').classList.add('hidden');
        document.getElementById('addDisciplineSection').classList.add('hidden');
        document.getElementById('studentBulletinContent').innerHTML = '<p style="text-align: center; font-style: italic;">Carregando boletim...</p>';
        if(studentPrintOptions) studentPrintOptions.classList.add('hidden'); // Hide student print options on logout
        document.getElementById('studentNotesContent').innerHTML = '<p class="no-notes">Nenhuma anotação disponível.</p>'; // NEW: Clear student notes
    }

    document.getElementById('loginButton').addEventListener('click', function() {
        const usernameInput = document.getElementById('username');
        const passwordInput = document.getElementById('password');
        const loginError = document.getElementById('loginError');
        const username = usernameInput.value;
        const password = passwordInput.value;
        loginError.textContent = '';
        let foundUser = users.find(user => user.username === username && user.password === password);
        if (foundUser) {
            currentUser = foundUser;
            // Only store non-sensitive data in localStorage for currentUser state
            localStorage.setItem('currentUser', JSON.stringify({ username: foundUser.username, role: foundUser.role }));
            loginContainer.classList.add('hidden');
            appContainer.classList.remove('hidden');
            if (currentUser.role === 'admin' || currentUser.role === 'coordenador') {
                showStudentManagementSection();
                userManagementSection.classList.remove('hidden');
                sidebarHr.classList.remove('hidden');
                professorAgendaButton.classList.add('hidden'); // NEW
            } else if (currentUser.role === 'professor') {
                showProfessorSection(currentUser);
                userManagementSection.classList.add('hidden');
                sidebarHr.classList.add('hidden');
                professorAgendaButton.classList.remove('hidden'); // NEW
            } else if (currentUser.role === 'aluno') {
                 // Find the corresponding student data
                const student = students.find(s => s.name.trim().toLowerCase() === currentUser.name.trim().toLowerCase()); // Assuming student name matches user name for linking
                if (student) {
                    currentUser.studentId = student.id; // Link student user to student data
                }
                showStudentBulletin();
                userManagementSection.classList.add('hidden');
                sidebarHr.classList.add('hidden');
                professorAgendaButton.classList.add('hidden'); // NEW
            }
        }
        else {
            loginError.textContent = 'Usuário ou senha inválidos.';
        }
    });

    window.addEventListener('load', function() {
        loadData(); // Load data first

        const savedUser = JSON.parse(localStorage.getItem('currentUser'));
        if (savedUser) {
            // Find the full user object from the loaded users data
            currentUser = users.find(user => user.username === savedUser.username && user.role === savedUser.role);
             if (currentUser && currentUser.role === 'aluno') {
                const student = students.find(s => s.name.trim().toLowerCase() === currentUser.name.trim().toLowerCase()); // Assuming student name matches user name for linking
                 if (student) {
                    currentUser.studentId = student.id; // Link student user to student data
                 } else {
                     console.error('Aluno data not found for logged in user:', currentUser.name);
                     // Optionally log out the user if student data is essential
                     logout();
                     return; // Stop further execution if student data is missing for an aluno user
                 }
            }
            if (currentUser) {
                appContainer.classList.remove('hidden');
                loginContainer.classList.add('hidden');
                if (currentUser.role === 'admin' || currentUser.role === 'coordenador') {
                    showStudentManagementSection();
                    userManagementSection.classList.remove('hidden');
                    sidebarHr.classList.remove('hidden');
                    professorAgendaButton.classList.add('hidden'); // NEW
                } else if (currentUser.role === 'professor') {
                    showProfessorSection(currentUser);
                    userManagementSection.classList.add('hidden');
                    sidebarHr.classList.add('hidden');
                    professorAgendaButton.classList.remove('hidden'); // NEW
                } else if (currentUser.role === 'aluno') {
                    showStudentBulletin();
                    userManagementSection.classList.add('hidden');
                    sidebarHr.classList.add('hidden');
                    professorAgendaButton.classList.add('hidden'); // NEW
                }
            } else {
                logout(); // Logout if saved user data is invalid or not found in loaded users
            }
        } else {
            loginContainer.classList.remove('hidden');
            appContainer.classList.add('hidden');
        }
    });


    function renderStudentTable(studentsToRender) {
        const tableBody = document.querySelector('#studentTable tbody');
        tableBody.innerHTML = '';
        if (!studentsToRender || studentsToRender.length === 0) {
            const colspan = 11; // Atualizado para 11 colunas
            const row = tableBody.insertRow();
            const cell = row.insertCell();
            cell.colSpan = colspan;
            cell.textContent = "Nenhum aluno encontrado.";
            cell.style.textAlign = 'center';
            cell.style.fontStyle = 'italic';
            return;
        }
        studentsToRender.forEach(student => {
            const sortedDisciplines = student.disciplines.sort((a, b) => a.unit - b.unit);
            if (sortedDisciplines.length === 0) {
                const row = tableBody.insertRow();
                row.classList.add('student-group-header');
                row.insertCell().textContent = student.name;
                row.insertCell().textContent = student.course;
                row.insertCell().textContent = student.class;
                const emptyCell = row.insertCell();
                emptyCell.colSpan = 8; // Nome, Curso, Turma são 3. Total 11. 11-3 = 8
                emptyCell.textContent = "Nenhuma disciplina adicionada.";
                emptyCell.style.textAlign = 'center';
                emptyCell.style.fontStyle = 'italic';
                row.insertCell().textContent = '';
            } else {
                sortedDisciplines.forEach((discipline, index) => {
                    const row = tableBody.insertRow();
                    row.classList.add('discipline-row');
                    if (index === 0) {
                        const nameCell = row.insertCell(); nameCell.textContent = student.name; nameCell.rowSpan = sortedDisciplines.length; nameCell.classList.add('student-group-header'); nameCell.style.verticalAlign = 'top';
                        const courseCell = row.insertCell(); courseCell.textContent = student.course; courseCell.rowSpan = sortedDisciplines.length; courseCell.classList.add('student-group-header'); courseCell.style.verticalAlign = 'top';
                        const classCell = row.insertCell(); classCell.textContent = student.class; classCell.rowSpan = sortedDisciplines.length; classCell.classList.add('student-group-header'); classCell.style.verticalAlign = 'top';
                    }
                    row.insertCell().textContent = discipline.discipline;
                    row.insertCell().textContent = discipline.unit;
                    ['eval1', 'eval2', 'finalGrade', 'situation', 'observation'].forEach(field => {
                        const cell = row.insertCell(); cell.classList.add('editable-cell');
                        cell.dataset.studentId = student.id; cell.dataset.disciplineName = discipline.discipline; cell.dataset.unitNumber = discipline.unit; cell.dataset.field = field;
                        if (field === 'observation') cell.dataset.type = 'text';
                        else {
                            cell.dataset.type = 'select';
                            if (field === 'eval1' || field === 'eval2') cell.dataset.options = ',"A","PA","ND"';
                            else if (field === 'finalGrade') cell.dataset.options = ',"D","ND"';
                            else if (field === 'situation') cell.dataset.options = ',"Aprovado","Reprovado","Pendente"';
                        }
                        cell.innerHTML = `<span>${discipline[field] || (field === 'observation' ? '' : '-')}</span>`;
                    });
                    const actionsCell = row.insertCell();
                    actionsCell.innerHTML = `<button type="button" class="button delete-button" data-student-id="${student.id}" data-discipline-name="${discipline.discipline}" data-unit-number="${discipline.unit}">Excluir</button>`;
                });
            }
        });
        attachEditableCellListeners('#studentTable');
        attachDeleteButtonListeners('#studentTable');
    }

     // Modified renderProfessorTable to use the new assignments structure
    function renderProfessorTable(studentsToRender, selectedDiscipline, selectedClass, selectedUnit) {
        const tableBody = document.querySelector('#professorStudentTable tbody');
        tableBody.innerHTML = '';
        const professorAssignments = currentUser.assignments || [];
        const assignedClassesForDiscipline = professorAssignments
            .filter(assignment => assignment.discipline === selectedDiscipline)
            .flatMap(assignment => assignment.classes);

        if (!studentsToRender || studentsToRender.length === 0 || !assignedClassesForDiscipline.includes(selectedClass)) {
             document.getElementById('professorNoStudentsMessage').classList.remove('hidden');
            document.querySelector('#professorStudentTable thead').classList.add('hidden'); // Hide header if no students
             return;
        } else {
             document.getElementById('professorNoStudentsMessage').classList.add('hidden');
             document.querySelector('#professorStudentTable thead').classList.remove('hidden'); // Show header if students are rendered
        }

        studentsToRender.forEach(student => {
            const disciplineData = student.disciplines.find(d => d.discipline === selectedDiscipline && d.unit == selectedUnit); // Use == for unit comparison due to string/number mix
            const row = tableBody.insertRow();
            row.insertCell().textContent = student.name;
            row.insertCell().textContent = student.course;
            row.insertCell().textContent = student.unit;
            ['eval1', 'eval2', 'finalGrade', 'situation', 'observation'].forEach(field => {
                const cell = row.insertCell(); cell.classList.add('editable-cell');
                cell.dataset.studentId = student.id; cell.dataset.disciplineName = selectedDiscipline; cell.dataset.unitNumber = selectedUnit; cell.dataset.field = field;
                if (field === 'observation') cell.dataset.type = 'text';
                else {
                    cell.dataset.type = 'select';
                    if (field === 'eval1' || field === 'eval2') cell.dataset.options = ',"A","PA","ND"';
                    else if (field === 'finalGrade') cell.dataset.options = ',"D","ND"';
                    else if (field === 'situation') cell.dataset.options = ',"Aprovado","Reprovado","Pendente"';
                }
                cell.innerHTML = `<span>${disciplineData ? (disciplineData[field] || (field === 'observation' ? '' : '-')) : (field === 'observation' ? '' : '-')}</span>`;
            });
        });
        attachEditableCellListeners('#professorStudentTable');
    }


    // Modified renderUsersTable to display new professor assignments structure
    function renderUsersTable(usersToRender) {
        const tableBody = document.querySelector('#usersTable tbody');
        tableBody.innerHTML = '';
        const noUsersMessage = document.getElementById('noUsersMessage');
        const displayUsers = usersToRender.filter(user => user.username !== 'administrador');
        if (!displayUsers || displayUsers.length === 0) {
            noUsersMessage.classList.remove('hidden');
            document.getElementById('usersTable').classList.add('hidden');
        } else {
            noUsersMessage.classList.add('hidden'); // Corrected logic: hide message if there are users
            document.getElementById('usersTable').classList.remove('hidden');
            displayUsers.forEach(user => {
                const row = tableBody.insertRow();
                row.insertCell().textContent = user.name;
                row.insertCell().textContent = user.username;
                row.insertCell().textContent = user.role.charAt(0).toUpperCase() + user.role.slice(1);

                // Display professor assignments based on the new structure
                if (user.role === 'professor' && user.assignments) {
                    const disciplinesText = user.assignments.map(assignment => assignment.discipline).join(', ');
                    // Concatenate discipline and classes for display in a more readable way
                    const classesText = user.assignments.map(assignment => `${assignment.discipline}: ${assignment.classes.join(', ')}`).join('; ');

                    row.insertCell().textContent = disciplinesText || '-';
                    row.insertCell().textContent = classesText || '-';
                } else {
                    row.insertCell().textContent = '-'; // No disciplines for non-professors
                    row.insertCell().textContent = '-'; // No classes for non-professors
                }

                row.insertCell().textContent = user.password; // Display password for demonstration (should be removed in real app)
                const actionsCell = row.insertCell();

                // Always show edit/delete buttons for professors and coordinators, and delete for students
                 if (user.role !== 'admin') { // Admin cannot be edited/deleted here
                    actionsCell.innerHTML = `<button type="button" class="button" onclick="editUser('${user.username}')">Editar</button> <button type="button" class="button delete-button" onclick="deleteUser('${user.username}')">Excluir</button>`;
                 } else {
                    actionsCell.textContent = '-';
                 }
                 // Note: Edit for 'aluno' is currently limited in the modal, but the button is shown.
            });
        }
    }

    function attachEditableCellListeners(tableSelector) {
        const table = document.querySelector(tableSelector);
        if (!table) return;
        table.querySelectorAll('.editable-cell').forEach(cell => {
            const newCell = cell.cloneNode(true);
            cell.parentNode.replaceChild(newCell, cell);
        });
        table.querySelectorAll('.editable-cell').forEach(cell => {
            cell.removeEventListener('click', activateEditableCell); // Prevent multiple listeners
            cell.addEventListener('click', activateEditableCell);
        });
    }

    function activateEditableCell(event) {
        const cell = event.target.closest('.editable-cell');
        if (!cell || cell.classList.contains('editing')) return;
        const isProfessorTable = cell.closest('#professorStudentTable');
        const isStudentTable = cell.closest('#studentTable');

        // Prevent editing based on user role and table
        if (currentUser.role === 'professor' && isStudentTable) return; // Professors can only edit their assigned students/disciplines via professor table
        if ((currentUser.role === 'admin' || currentUser.role === 'coordenador') && isProfessorTable) return; // Admins/Coordinators can edit via student table
        if (currentUser.role === 'aluno') return; // Students cannot edit

         // Additional check for professor: ensure the selected cell's discipline/class is assigned to the professor
        if (currentUser.role === 'professor' && isProfessorTable) {
            const disciplineName = cell.dataset.disciplineName;
            const classMatch = currentUser.assignments.some(assignment =>
                assignment.discipline === disciplineName &&
                assignment.classes.includes(document.getElementById('professorClassSelect').value) // Check against the selected class in the professor view
            );
            if (!classMatch) {
                 alert('Você não está atribuído a esta disciplina nesta turma.'); // Optional: provide feedback
                return; // Prevent editing if not assigned
            }
        }


        cell.classList.add('editing');
        const currentValue = cell.querySelector('span') ? cell.querySelector('span').textContent.trim() : cell.textContent.trim(); // Get value from span if exists
        const studentId = cell.dataset.studentId;
        const disciplineName = cell.dataset.disciplineName;
        const unitNumber = parseInt(cell.dataset.unitNumber);
        const field = cell.dataset.field;
        const inputType = cell.dataset.type;
        const options = cell.dataset.options ? cell.dataset.options.split(',') : [];
        let inputElement;
        if (inputType === 'select') {
            inputElement = document.createElement('select');
            options.forEach(optionValue => {
                const option = document.createElement('option');
                option.value = optionValue;
                if (field === 'finalGrade') option.textContent = optionValue === 'D' ? 'Desenvolveu (D)' : (optionValue === 'ND' ? 'Não Desenvolveu (ND)' : (optionValue === '' ? ' - ' : optionValue));
                else option.textContent = optionValue || (optionValue === '' ? ' - ' : optionValue);
                if (currentValue === optionValue || (currentValue === '-' && optionValue === '') || (field === 'finalGrade' && currentValue.includes(optionValue))) option.selected = true;
                inputElement.appendChild(option);
            });
        } else if (inputType === 'text') {
            inputElement = document.createElement('input'); inputElement.type = 'text'; inputElement.value = currentValue;
        } else { cell.classList.remove('editing'); return; }
        cell.innerHTML = ''; cell.appendChild(inputElement); inputElement.focus();
        inputElement.addEventListener('blur', saveEditableCell);
        inputElement.addEventListener('keypress', function(e) { if (e.key === 'Enter') saveEditableCell(e); });
    }

    function saveEditableCell(event) {
        const inputElement = event.target;
        const cell = inputElement.closest('.editable-cell');
        if (!cell || !cell.classList.contains('editing')) return;
        const newValue = inputElement.value.trim();
        const studentId = cell.dataset.studentId;
        const disciplineName = cell.dataset.disciplineName;
        const unitNumber = parseInt(cell.dataset.unitNumber);
        const field = cell.dataset.field;
        const student = students.find(s => s.id === studentId);
        if (student) {
            let disciplineEntry = student.disciplines.find(d => d.discipline === disciplineName && d.unit === unitNumber);
            if (!disciplineEntry) {
                disciplineEntry = { discipline: disciplineName, unit: unitNumber, eval1: '', eval2: '', finalGrade: '', situation: 'Pendente', observation: '' };
                student.disciplines.push(disciplineEntry);
            }
            disciplineEntry[field] = newValue;
            let displayValue = newValue || '';
            if (field === 'finalGrade') displayValue = newValue === 'D' ? 'Desenvolveu (D)' : (newValue === 'ND' ? 'Não Desenvolveu (ND)' : (newValue === '' ? ' - ' : newValue));
            else if ((field === 'eval1' || field === 'eval2' || field === 'situation') && newValue === '') displayValue = ' - ';
            cell.innerHTML = `<span>${displayValue}</span>`;
             saveData(); // Save data after editing a cell
        } else {
            console.error('Erro: Aluno não encontrado para edição.'); cell.innerHTML = `<span>${inputElement.defaultValue}</span>`; // Revert if error
        }
        cell.classList.remove('editing');
    }

    // Modified populateProfessorSelects to use the new assignments structure
    function populateProfessorSelects(user) {
        const disciplineSelect = document.getElementById('professorDisciplineSelect');
        const classSelect = document.getElementById('professorClassSelect');
        const unitSelect = document.getElementById('professorUnitSelect');

        disciplineSelect.innerHTML = '<option value="">Selecione a Disciplina</option>';
        classSelect.innerHTML = '<option value="">Selecione a Turma</option>';
         unitSelect.value = ''; // Reset unit selection

        if (user && user.assignments) {
            // Populate discipline select based on assigned disciplines
            const uniqueDisciplines = [...new Set(user.assignments.map(assignment => assignment.discipline))].sort();
            uniqueDisciplines.forEach(discipline => { const option = document.createElement('option'); option.value = discipline; option.textContent = discipline; disciplineSelect.appendChild(option); });

            // Add event listener to discipline select to populate class select
            disciplineSelect.removeEventListener('change', updateProfessorClassSelect);
            disciplineSelect.addEventListener('change', updateProfessorClassSelect);

            // Initial population of class select based on the default (or first) selected discipline
            updateProfessorClassSelect();

        }

        disciplineSelect.removeEventListener('change', loadProfessorTable);
        classSelect.removeEventListener('change', loadProfessorTable);
        unitSelect.removeEventListener('change', loadProfessorTable);

        disciplineSelect.addEventListener('change', loadProfessorTable);
        classSelect.addEventListener('change', loadProfessorTable);
        unitSelect.addEventListener('change', loadProfessorTable);
    }

    // New function to update the class select based on the selected discipline for the professor
    function updateProfessorClassSelect() {
        const disciplineSelect = document.getElementById('professorDisciplineSelect');
        const classSelect = document.getElementById('professorClassSelect');
        const selectedDiscipline = disciplineSelect.value;

        classSelect.innerHTML = '<option value="">Selecione a Turma</option>';
        classSelect.disabled = true; // Disable by default

        if (currentUser && currentUser.assignments && selectedDiscipline) {
            const assignment = currentUser.assignments.find(a => a.discipline === selectedDiscipline);
            if (assignment && assignment.classes.length > 0) {
                assignment.classes.sort().forEach(className => {
                    const option = document.createElement('option');
                    option.value = className;
                    option.textContent = className;
                    classSelect.appendChild(option);
                });
                classSelect.disabled = false;
            }
        }
         // Also clear the table and show message when discipline changes
        document.querySelector('#professorStudentTable tbody').innerHTML = '';
        document.getElementById('professorNoStudentsMessage').classList.remove('hidden');
         document.querySelector('#professorStudentTable thead').classList.add('hidden'); // Hide table header
    }


     // Modified loadProfessorTable to use the new assignments structure and selected class
    function loadProfessorTable() {
        const selectedDiscipline = document.getElementById('professorDisciplineSelect').value;
        const selectedClass = document.getElementById('professorClassSelect').value;
        const selectedUnit = document.getElementById('professorUnitSelect').value;

        // Check if the professor is assigned to this discipline in this class
        const isAssigned = currentUser && currentUser.assignments &&
                           currentUser.assignments.some(assignment =>
                               assignment.discipline === selectedDiscipline &&
                               assignment.classes.includes(selectedClass)
                           );

        if (selectedDiscipline && selectedClass && selectedUnit && isAssigned) {
            const studentsToDisplay = students.filter(student => student.class === selectedClass);
            renderProfessorTable(studentsToDisplay, selectedDiscipline, selectedClass, parseInt(selectedUnit));
        } else {
            document.querySelector('#professorStudentTable tbody').innerHTML = '';
            document.getElementById('professorNoStudentsMessage').classList.remove('hidden');
             document.querySelector('#professorStudentTable thead').classList.add('hidden'); // Hide table header
        }
    }


    function addStudent() {
        const studentNameInput = document.getElementById('studentName'); const classSelect = document.getElementById('class');
        const name = studentNameInput.value.trim(); const className = classSelect.value; const classInfo = getClassInfo(className);
        if (!name || !className || classInfo.unit === 'Desconhecido') { alert('Por favor, preencha o Nome do Aluno e selecione a Turma.'); return; }

        // Generate a simple unique ID (timestamp + random number)
        const newStudentId = 's' + Date.now() + Math.floor(Math.random() * 1000);

        const newStudent = { id: newStudentId, name: name, course: classInfo.course, class: className, unit: classInfo.unit, disciplines: [], professorNotes: [] }; // NEW: Initialize professorNotes
        students.push(newStudent); studentNameInput.value = ''; classSelect.value = '';
        saveData(); // Save data after adding a student
        renderStudentTable(students);
        populateClassSelect(document.getElementById('disciplineClassSelect')); populateStudentSelectForDiscipline();
        populateClassSelect(document.getElementById('printClassSelect')); populateStudentPrintSelect();
        if (currentUser && currentUser.role === 'professor') loadProfessorTable();
        alert('Aluno adicionado com sucesso!');
    }

    function addDiscipline() {
        const disciplineClassSelect = document.getElementById('disciplineClassSelect'); const studentSelect = document.getElementById('studentSelect');
        const disciplineSelect = document.getElementById('disciplineSelect'); const unitSelectDiscipline = document.getElementById('unitSelect');
        const evaluation1Select = document.getElementById('evaluation1'); const evaluation2Select = document.getElementById('evaluation2');
        const finalGradeSelect = document.getElementById('finalGrade'); const observationInput = document.getElementById('observationInput');
        const selectedClass = disciplineClassSelect.value; const studentId = studentSelect.value; const disciplineName = disciplineSelect.value;
        const unitNumber = parseInt(unitSelectDiscipline.value); const eval1 = evaluation1Select.value; const eval2 = evaluation2Select.value;
        const finalGrade = finalGradeSelect.value; const observation = observationInput.value.trim();
        if (!selectedClass || !studentId || !disciplineName || !unitNumber || !eval1 || !eval2 || !finalGrade) { alert('Por favor, selecione a Turma, o Aluno e preencha todos os campos da disciplina (exceto Observações).'); return; }
        const student = students.find(s => s.id === studentId);
        if (student) {
            if (student.class !== selectedClass) { alert('Erro: O aluno selecionado não pertence à turma escolhida.'); return; }
            const existingDiscipline = student.disciplines.find(d => d.discipline === disciplineName && d.unit === unitNumber);
            if (existingDiscipline) { alert(`A disciplina "${disciplineName}" para a Unidade ${unitNumber} já existe para este aluno.`); return; }
            let situation = 'Pendente'; if (finalGrade === 'D') situation = 'Aprovado'; else if (finalGrade === 'ND') situation = 'Reprovado';
            const newDiscipline = { discipline: disciplineName, unit: unitNumber, eval1: eval1, eval2: eval2, finalGrade: finalGrade, situation: situation, observation: observation };
            student.disciplines.push(newDiscipline);
            disciplineClassSelect.value = ''; populateStudentSelectForDiscipline(); disciplineSelect.value = ''; unitSelectDiscipline.value = ''; evaluation1Select.value = ''; evaluation2Select.value = ''; finalGradeSelect.value = ''; observationInput.value = '';
            saveData(); // Save data after adding a discipline
            renderStudentTable(students);
            if (currentUser && currentUser.role === 'professor') loadProfessorTable();
            if (currentUser && currentUser.role === 'aluno' && currentUser.studentId === student.id) showStudentBulletin();
            alert('Disciplina adicionada com sucesso!');
        } else { alert('Aluno não encontrado.'); }
    }

    function populateStudentSelectForDiscipline(className = '') {
        const studentSelect = document.getElementById('studentSelect'); studentSelect.innerHTML = '';
        const defaultOption = document.createElement('option'); defaultOption.value = ""; defaultOption.textContent = className ? "Carregando alunos..." : "Selecione a Turma Primeiro";
        studentSelect.appendChild(defaultOption); studentSelect.disabled = true;
        setTimeout(() => {
            const studentsToDisplay = className ? students.filter(student => student.class === className) : []; studentSelect.innerHTML = '';
            const fragment = document.createDocumentFragment();
            if (studentsToDisplay.length > 0) {
                const selectStudentOption = document.createElement('option'); selectStudentOption.value = ""; selectStudentOption.textContent = "Selecione o Aluno"; fragment.appendChild(selectStudentOption);
                studentsToDisplay.forEach(student => { const option = document.createElement('option'); option.value = student.id; option.textContent = student.name; fragment.appendChild(option); });
                studentSelect.disabled = false;
            } else {
                const noStudentsOption = document.createElement('option'); noStudentsOption.value = ""; noStudentsOption.textContent = "Nenhum aluno encontrado para a Turma selecionada"; fragment.appendChild(noStudentsOption); studentSelect.disabled = true;
            }
            studentSelect.appendChild(fragment);
        }, 50);
    }

    function populateStudentPrintSelect(className = '') {
        const studentSelect = document.getElementById('studentSelectPrint'); studentSelect.innerHTML = '';
        const defaultOption = document.createElement('option'); defaultOption.value = ""; defaultOption.textContent = className ? "Carregando alunos..." : "Selecione a Turma Primeiro";
        studentSelect.appendChild(defaultOption); studentSelect.disabled = true;
        setTimeout(() => {
            const studentsToDisplay = className ? students.filter(student => student.class === className) : []; studentSelect.innerHTML = '';
            const fragment = document.createDocumentFragment();
            if (studentsToDisplay.length > 0) {
                const selectStudentOption = document.createElement('option'); selectStudentOption.value = ""; selectStudentOption.textContent = "Selecione o Aluno"; fragment.appendChild(selectStudentOption);
                studentsToDisplay.forEach(student => { const option = document.createElement('option'); option.value = student.id; option.textContent = `${student.name} - ${student.unit}`; fragment.appendChild(option); });
                studentSelect.disabled = false;
            } else {
                const noStudentsOption = document.createElement('option'); noStudentsOption.value = ""; noStudentsOption.textContent = "Selecione a Turma Primeiro"; fragment.appendChild(noStudentsOption); studentSelect.disabled = true;
            }
            studentSelect.appendChild(fragment);
        }, 50);
    }

    function populateClassSelect(selectElement) {
        selectElement.innerHTML = '<option value="">Selecione a Turma</option>';
        const uniqueClasses = [...new Set(students.map(student => student.class))].sort();
        uniqueClasses.forEach(className => { const option = document.createElement('option'); option.value = className; option.textContent = className; selectElement.appendChild(option); });
    }

    function printAllReports() {
        // This function will now print ALL students from ALL classes, respecting unit and report type filters
        const selectedUnits = Array.from(document.querySelectorAll('#printControls .unitCheckbox:checked')).map(cb => parseInt(cb.value));
        const reportType = document.querySelector('#printControls input[name="reportType"]:checked').value;

        if (selectedUnits.length === 0) {
            alert('Por favor, selecione pelo menos uma unidade para imprimir.');
            return;
        }

        let allBulletinsHTML = '';
        students.forEach(student => {
            allBulletinsHTML += generateBulletinHTML(student, selectedUnits, reportType, true); // Pass true to include notes
        });

        if (allBulletinsHTML === '') {
            alert('Nenhum dado de boletim encontrado para os filtros selecionados.');
            return;
        }
        openPrintWindow(allBulletinsHTML, "Boletins de Todos os Alunos");
    }


    function printStudentReport() {
        const printClassSelect = document.getElementById('printClassSelect');
        const studentSelect = document.getElementById('studentSelectPrint');
        const selectedUnits = Array.from(document.querySelectorAll('#printControls .unitCheckbox:checked')).map(cb => parseInt(cb.value));
        const reportType = document.querySelector('#printControls input[name="reportType"]:checked').value;
        const studentId = studentSelect.value;

        if (!studentId) { alert('Por favor, selecione o Aluno para imprimir.'); return; }
        if (selectedUnits.length === 0) { alert('Por favor, selecione pelo menos uma unidade.'); return;}

        const student = students.find(s => s.id === studentId);
        if (!student) { alert('Aluno não encontrado para impressão.'); return; }

        const bulletinHTML = generateBulletinHTML(student, selectedUnits, reportType, true); // Pass true to include notes
        openPrintWindow(bulletinHTML, `Boletim de ${student.name}`);
    }

    // NOVA FUNÇÃO para imprimir boletins da turma
    function printClassReports() {
        const printClassSelect = document.getElementById('printClassSelect');
        const selectedClass = printClassSelect.value;
        const selectedUnits = Array.from(document.querySelectorAll('#printControls .unitCheckbox:checked')).map(cb => parseInt(cb.value));
        const reportType = document.querySelector('#printControls input[name="reportType"]:checked').value;

        if (!selectedClass) {
            alert('Por favor, selecione a Turma para imprimir.');
            return;
        }
        if (selectedUnits.length === 0) {
            alert('Por favor, selecione pelo menos uma unidade para imprimir.');
            return;
        }

        const studentsInClass = students.filter(student => student.class === selectedClass);

        if (studentsInClass.length === 0) {
            alert(`Nenhum aluno encontrado na turma ${selectedClass}.`);
            return;
        }

        let classBulletinsHTML = '';
        studentsInClass.forEach(student => {
            classBulletinsHTML += generateBulletinHTML(student, selectedUnits, reportType, true); // Pass true to include notes
        });

        if (classBulletinsHTML === '') {
            alert(`Nenhum dado de boletim encontrado para a turma ${selectedClass} com os filtros selecionados.`);
            return;
        }
        openPrintWindow(classBulletinsHTML, `Boletins da Turma ${selectedClass}`);
    }


    // MODIFICADA: generateBulletinHTML para incluir anotações
    function generateBulletinHTML(student, unitsToInclude, reportType = 'full', includeProfessorNotes = false) {
        let bulletinHTML = `<div class="student-report"><div class="bulletin-header"><p class="line1"><strong>SENAC-PAULISTA</strong> - 2025</p><p class="line2">Boletim Escolar - MÉDIOTEC</p><p class="line3"><strong>Aluno:</strong> ${student.name} - <strong>Turma:</strong> ${student.class} - <strong>Turno:</strong> ${student.unit} - <strong>Curso:</strong> ${student.course}</p></div><table class="bulletin-table"><thead><tr><th>Disciplina</th><th>Unidade</th>${reportType === 'full' ? '<th>Avaliação 1</th><th>Avaliação 2</th>' : ''}<th>Menção Final</th><th>Situação</th><th>Observações</th></tr></thead><tbody>`;
        const disciplinesToPrint = student.disciplines.filter(d => unitsToInclude.includes(d.unit)).sort((a, b) => { if (a.unit !== b.unit) return a.unit - b.unit; return a.discipline.localeCompare(b.discipline); });
        if (disciplinesToPrint.length === 0) {
            const colspan = reportType === 'full' ? 7 : 5; // Ajustado para 7 ou 5 colunas
            bulletinHTML += `<tr><td colspan="${colspan}">Nenhuma disciplina ou nota encontrada para as unidades selecionadas.</td></tr>`;
        } else {
            disciplinesToPrint.forEach(discipline => {
                bulletinHTML += `<tr><td>${discipline.discipline}</td><td>${discipline.unit}</td>${reportType === 'full' ? `<td>${discipline.eval1 || '-'}</td><td>${discipline.eval2 || '-'}</td>` : ''}<td>${discipline.finalGrade || '-'}</td><td>${discipline.situation || '-'}</td><td>${discipline.observation || ''}</td></tr>`;
            });
        }
        bulletinHTML += `</tbody></table>`;

        // NEW: Add professor notes to the print output if requested
        if (includeProfessorNotes && student.professorNotes && student.professorNotes.length > 0) {
            const notesForPrint = student.professorNotes.filter(note => unitsToInclude.includes(note.unit)).sort((a, b) => {
                // Sort by date (descending), then by discipline, then by professor name
                const dateA = new Date(a.date);
                const dateB = new Date(b.date);
                if (dateA.getTime() !== dateB.getTime()) return dateB.getTime() - dateA.getTime();
                if (a.discipline !== b.discipline) return a.discipline.localeCompare(b.discipline);
                return a.professorName.localeCompare(b.professorName);
            });

            if (notesForPrint.length > 0) {
                bulletinHTML += `<div class="professor-notes-print"><h4>Anotações dos Professores:</h4>`;
                notesForPrint.forEach(note => {
                    bulletinHTML += `<div class="professor-note-item">
                                        <p><strong>Disciplina:</strong> ${note.discipline}, <strong>Unidade:</strong> ${note.unit}</p>
                                        <p><strong>Anotação:</strong> ${note.content}</p>
                                        <span class="note-meta">Feita por: ${note.professorName} em ${new Date(note.date).toLocaleDateString()}</span>
                                     </div>`;
                });
                bulletinHTML += `</div>`;
            }
        }

        bulletinHTML += `</div>`; // Close student-report div
        return bulletinHTML;
    }

    // Helper para abrir janela de impressão
    function openPrintWindow(contentHTML, title) {
        const printWindow = window.open('', '_blank');
        printWindow.document.open();
        printWindow.document.write(`
            <!DOCTYPE html>
            <html><head><title>${title}</title>
                <style>
                    body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; margin: 1.5cm; font-size: 10pt; }
                    .student-report { page-break-inside: avoid; page-break-after: always; margin-bottom: 0; padding: 10px 0;}
                    .student-report:last-child { page-break-after: avoid; }
                    .bulletin-header { text-align: center; margin-bottom: 20px; padding-bottom: 15px; border-bottom: 2px solid #000; }
                    .bulletin-header p { margin: 5px 0; font-size: 11pt; color: #333; } .bulletin-header p strong { font-weight: bold; }
                    .bulletin-header .line1 { font-size: 18pt; font-weight: bold; margin-bottom: 8px; }
                    .bulletin-header .line2 { font-size: 14pt; margin-bottom: 8px; }
                    .bulletin-header .line3 { font-size: 11pt; margin-top: 8px; margin-bottom: 0; }
                    .bulletin-table { width: 100%; border-collapse: collapse; margin-top: 15px; }
                    .bulletin-table th, .bulletin-table td { border: 1px solid #000; padding: 8px; text-align: center; font-size: 9pt; }
                    .bulletin-table th { background-color: #eee !important; color: #000 !important; font-weight: bold; }
                    .bulletin-table td:first-child { text-align: left; } .bulletin-table td:last-child { text-align: left; }
                    @page { size: A4; margin: 1.5cm; }
                    /* New print styles for professor notes */
                    .professor-notes-print { margin-top: 25px; padding-top: 15px; border-top: 1px solid #ccc; }
                    .professor-notes-print h4 { font-size: 12pt; margin-bottom: 10px; color: #000; }
                    .professor-note-item { border: 1px solid #eee; padding: 8px; margin-bottom: 8px; background-color: #f8f8f8; page-break-inside: avoid; }
                    .professor-note-item p { margin: 0; line-height: 1.4; }
                    .professor-note-item strong { font-weight: bold; }
                    .professor-note-item span { font-size: 9pt; color: #666; display: block; margin-top: 5px; }
                </style>
            </head><body>${contentHTML}</body></html>`);
        printWindow.document.close();
        printWindow.print();
    }


    // Modified showAddProfessorForm to populate the new assignment structure UI
    function showAddProfessorForm() {
        populateProfessorAssignmentCheckboxes(document.getElementById('newProfessorAssignments'));
        document.getElementById('addProfessorModal').classList.remove('hidden');
    }

    function closeAddProfessorModal() {
        document.getElementById('addProfessorModal').classList.add('hidden');
        document.getElementById('newProfessorNameInput').value = '';
        document.getElementById('newProfessorUsernameInput').value = '';
        document.getElementById('newProfessorPasswordInput').value = '';
        document.getElementById('newProfessorAssignments').innerHTML = ''; // Clear the assignment checkboxes
    }

    // Modified saveProfessor to capture the new assignment structure
    function saveProfessor() {
        const name = document.getElementById('newProfessorNameInput').value.trim();
        const username = document.getElementById('newProfessorUsernameInput').value.trim();
        const password = document.getElementById('newProfessorPasswordInput').value;

        if (!name || !username || !password) { alert('Por favor, preencha Nome, Usuário e Senha para o professor.'); return; }
        if (users.some(user => user.username === username)) { alert('Nome de usuário já existe. Por favor, escolha outro.'); return; }

        // Capture assignments from the new UI structure
        const assignments = [];
        document.querySelectorAll('#newProfessorAssignments .discipline-assignment').forEach(disciplineDiv => {
            const discipline = disciplineDiv.dataset.discipline;
            const selectedClasses = Array.from(disciplineDiv.querySelectorAll('.class-checkboxes input[type="checkbox"]:checked')).map(cb => cb.value);
            if (selectedClasses.length > 0) {
                assignments.push({ discipline: discipline, classes: selectedClasses });
            }
        });

        users.push({ username: username, password: password, role: 'professor', name: name, assignments: assignments });
        saveData(); // Save data after adding a professor
        closeAddProfessorModal();
        renderUsersTable(users);
        alert('Professor adicionado com sucesso!');
    }

    function showAddCoordenadorForm() { document.getElementById('addCoordinatorModal').classList.remove('hidden'); }
    function closeAddCoordenadorModal() { document.getElementById('addCoordinatorModal').classList.add('hidden'); document.getElementById('newCoordinatorNameInput').value = ''; document.getElementById('newCoordinatorUsernameInput').value = ''; document.getElementById('newCoordinatorPasswordInput').value = ''; }
    function saveCoordinator() {
        const name = document.getElementById('newCoordinatorNameInput').value.trim(); const username = document.getElementById('newCoordinatorUsernameInput').value.trim(); const password = document.getElementById('newCoordinatorPasswordInput').value;
        if (!name || !username || !password) { alert('Por favor, preencha Nome, Usuário e Senha para o coordenador.'); return; }
        if (users.some(user => user.username === username)) { alert('Nome de usuário já existe. Por favor, escolha outro.'); return; }
        users.push({ username: username, password: password, role: 'coordenador', name: name, assignments: [] }); // Coordinators have empty assignments
        saveData(); // Save data after adding a coordinator
        closeAddCoordenadorModal(); renderUsersTable(users); alert('Coordenador adicionado com sucesso!');
    }
    function showManageUsersSection() { showSection(manageUsersSection); renderUsersTable(users); }

    // Modified editUser to populate the new assignment structure UI
    function editUser(username) {
        const user = users.find(u => u.username === username); if (!user) { alert('Usuário não encontrado para edição.'); return; }
        document.getElementById('editUserModalTitle').textContent = `Editar Usuário: ${user.name}`; document.getElementById('editUserOriginalUsername').value = user.username; document.getElementById('editUserRole').value = user.role;
        document.getElementById('editUserNameInput').value = user.name; document.getElementById('editUserUsernameInput').value = user.username; document.getElementById('editUserPasswordInput').value = '';

        const professorAssignmentsDiv = document.getElementById('editProfessorAssignments');
        if (user.role === 'professor') {
            professorAssignmentsDiv.classList.remove('hidden');
            populateProfessorAssignmentCheckboxes(professorAssignmentsDiv, user.assignments); // Populate with existing assignments
        } else {
            professorAssignmentsDiv.classList.add('hidden');
             professorAssignmentsDiv.innerHTML = ''; // Clear assignments if not a professor
        }

         // Hide assignments for student users in the edit modal (redundant with above but keeps logic clear)
        if (user.role === 'aluno') { professorAssignmentsDiv.classList.add('hidden'); professorAssignmentsDiv.innerHTML = ''; }

        document.getElementById('editUserModal').classList.remove('hidden');
    }

    function closeEditUserModal() {
        document.getElementById('editUserModal').classList.add('hidden');
        document.getElementById('editUserOriginalUsername').value = '';
        document.getElementById('editUserRole').value = '';
        document.getElementById('editUserNameInput').value = '';
        document.getElementById('editUserUsernameInput').value = '';
        document.getElementById('editUserPasswordInput').value = '';
        document.getElementById('editProfessorAssignments').innerHTML = ''; // Clear assignment checkboxes
    }

    // Modified updateUser to save the new assignment structure
    function updateUser() {
        const originalUsername = document.getElementById('editUserOriginalUsername').value;
        const userRole = document.getElementById('editUserRole').value;
        const newName = document.getElementById('editUserNameInput').value.trim();
        const newUsername = document.getElementById('editUserUsernameInput').value.trim();
        const newPassword = document.getElementById('editUserPasswordInput').value;

        if (!newName || !newUsername) { alert('Nome e Usuário não podem ser vazios.'); return; }
        if (newUsername !== originalUsername && users.some(user => user.username === newUsername)) { alert('Nome de usuário já existe. Por favor, escolha outro.'); return; }

        const userIndex = users.findIndex(u => u.username === originalUsername); if (userIndex === -1) { alert('Erro: Usuário original não encontrado.'); return; }

        users[userIndex].name = newName; users[userIndex].username = newUsername; if (newPassword) users[userIndex].password = newPassword;

        // Update assignments based on the new UI structure if the user is a professor
        if (userRole === 'professor') {
            const updatedAssignments = [];
            document.querySelectorAll('#editProfessorAssignments .discipline-assignment').forEach(disciplineDiv => {
                const discipline = disciplineDiv.dataset.discipline;
                const selectedClasses = Array.from(disciplineDiv.querySelectorAll('.class-checkboxes input[type="checkbox"]:checked')).map(cb => cb.value);
                 if (selectedClasses.length > 0) {
                    updatedAssignments.push({ discipline: discipline, classes: selectedClasses });
                 }
            });
             users[userIndex].assignments = updatedAssignments;
        }
         // For other roles, assignments remain empty or unchanged

        saveData(); // Save data after updating a user
        closeEditUserModal();
        renderUsersTable(users);

        if (currentUser && currentUser.username === originalUsername) {
            currentUser = users[userIndex];
            // Update studentId link if username changed for an aluno user
            if (currentUser.role === 'aluno') {
                 const student = students.find(s => s.name.trim().toLowerCase() === currentUser.name.trim().toLowerCase());
                 if (student) {
                    currentUser.studentId = student.id;
                 } else {
                     console.error('Could not relink student data after username update for user:', currentUser.username);
                      // Decide how to handle this - maybe revert the username change or show an error
                 }
                 showStudentBulletin(); // Refresh bulletin for the student
            } else if (currentUser.role === 'admin' || currentUser.role === 'coordenador') {
                showStudentManagementSection(); // Refresh student management for admin/coordinator
            } else if (currentUser.role === 'professor') {
                 showProfessorSection(currentUser); // Refresh professor section for professor
            }
        }
        alert('Usuário atualizado com sucesso!');
    }

    function deleteUser(username) {
        if (username === 'administrador') { alert('Não é possível excluir o usuário administrador principal.'); return; }
        if (currentUser && currentUser.username === username) { alert('Você não pode excluir seu próprio usuário.'); return; }
        // Removed the explicit check preventing deletion of 'aluno' users
        if (confirm(`Tem certeza que deseja excluir o usuário "${username}"? Esta ação não pode ser desfeita."`)) {
            const initialUserCount = users.length; users = users.filter(user => user.username !== username);
            if (users.length < initialUserCount) {
                 saveData(); // Save data after deleting a user
                 renderUsersTable(users);
                 alert('Usuário excluído com sucesso.');
                 // If the deleted user was the current user, log out
                 if (currentUser && currentUser.username === username) {
                     logout();
                 }
             } else { alert('Erro ao excluir usuário: Usuário não encontrado.'); }
        }
    }
    function searchStudent() {
        const searchTerm = document.getElementById('searchName').value.toLowerCase();
        if (searchTerm.trim() === '') renderStudentTable(students);
        else renderStudentTable(students.filter(student => student.name.toLowerCase().includes(searchTerm)));
    }
    function exportData() {
         const data = { students: students, users: users };
         const dataStr = JSON.stringify(data, null, 2); // Pretty print JSON
         const blob = new Blob([dataStr], { type: 'application/json' });
         const url = URL.createObjectURL(blob);
         const a = document.createElement('a');
         a.href = url;
         a.download = 'senac_boletim_backup_' + new Date().toISOString().slice(0,10) + '.json'; // Filename with date
         document.body.appendChild(a); // Required for Firefox
         a.click();
         document.body.removeChild(a);
         URL.revokeObjectURL(url); // Clean up
         alert('Dados exportados com sucesso!');
    }

    function importData() {
         const fileInput = document.getElementById('importFile');
         const file = fileInput.files[0];
         const importStatus = document.getElementById('importStatus');
         const performImportButton = document.getElementById('performImportButton');

         importStatus.textContent = ''; // Clear previous status

         if (!file) {
             importStatus.textContent = 'Por favor, selecione um arquivo para importar.';
             return;
         }

         const reader = new FileReader();
         reader.onload = function(event) {
             try {
                 const importedData = JSON.parse(event.target.result);
                 if (importedData.students && importedData.users) {
                     // Basic validation: check if arrays exist
                     if (Array.isArray(importedData.students) && Array.isArray(importedData.users)) {
                          // More detailed validation could be added here (e.g., check for required fields in objects)

                          // Check if the imported data contains an admin user
                          const importedAdmin = importedData.users.find(user => user.username === 'administrador' && user.role === 'admin');

                          if (!importedAdmin) {
                             importStatus.textContent = 'Erro de importação: O arquivo não contém um usuário administrador válido.';
                             performImportButton.disabled = true; // Disable import again on error
                             document.getElementById('importFileName').textContent = 'Nenhum arquivo selecionado.';
                             fileInput.value = ''; // Clear file input
                             return;
                          }

                          // Keep the existing admin password for security (don't overwrite with imported password)
                          const existingAdmin = users.find(user => user.username === 'administrador' && user.role === 'admin');
                          if (existingAdmin) {
                              // Find the imported admin user and update its password to the existing one
                              const importedAdminUser = importedData.users.find(user => user.username === 'administrador' && user.role === 'admin');
                              if(importedAdminUser) {
                                 importedAdminUser.password = existingAdmin.password;
                                 // Also ensure its assignments are an array if missing
                                 if (!Array.isArray(importedAdminUser.assignments)) importedAdminUser.assignments = [];
                              }
                          } else {
                              // If somehow the existing admin is missing, add the imported one but issue a warning
                              alert('Aviso: O usuário administrador existente foi substituído pelo do arquivo importado. A senha padrão será usada.');
                               if (!Array.isArray(importedData.users[0].assignments)) importedData.users[0].assignments = []; // Ensure assignments is array for imported admin
                          }


                          students = importedData.students;
                          users = importedData.users;

                          // Ensure all imported users have an assignments array if they are professors or if the field is missing
                           students = students.map(s => { // NEW: Ensure professorNotes for students
                               if (!Array.isArray(s.professorNotes)) s.professorNotes = [];
                               return s;
                           });

                           users = users.map(user => {
                               if (user.role === 'professor' && !Array.isArray(user.assignments)) {
                                   user.assignments = []; // Default to empty array if missing for professor
                               } else if (!Array.isArray(user.assignments)) {
                                   user.assignments = []; // Ensure assignments is an array for all users
                               }
                               return user;
                           });

                          saveData(); // Save the imported data
                          importStatus.textContent = 'Dados importados com sucesso!';
                          // Refresh UI based on the current user and imported data
                          if (currentUser) {
                               const updatedCurrentUser = users.find(user => user.username === currentUser.username && user.role === currentUser.role);
                               if (updatedCurrentUser) {
                                   currentUser = updatedCurrentUser; // Update currentUser reference
                                    if (currentUser.role === 'aluno') {
                                         const student = students.find(s => s.name.trim().toLowerCase() === currentUser.name.trim().toLowerCase());
                                         if (student) {
                                            currentUser.studentId = student.id;
                                         } else {
                                             console.warn('Could not relink student data after import for user:', currentUser.username);
                                              // Decide how to handle this - maybe show a specific message or log out
                                         }
                                         showStudentBulletin(); // Refresh bulletin for the student
                                    } else if (currentUser.role === 'admin' || currentUser.role === 'coordenador') {
                                         showStudentManagementSection(); // Refresh student management for admin/coordinator
                                    } else if (currentUser.role === 'professor') {
                                         showProfessorSection(currentUser); // Refresh professor section for professor
                                    }
                               } else {
                                    // Current user no longer exists in imported data, force logout
                                    alert('Seu usuário não foi encontrado nos dados importados. Você será desconectado.');
                                    logout();
                               }
                          } else {
                                // If no user was logged in, just render the tables with new data
                                 renderStudentTable(students);
                                 renderUsersTable(users);
                          }

                     } else {
                          importStatus.textContent = 'Erro de importação: Formato de dados inválido.';
                           performImportButton.disabled = true; // Disable import again on error
                           document.getElementById('importFileName').textContent = 'Nenhum arquivo selecionado.';
                           fileInput.value = ''; // Clear file input
                     }
                 } else {
                     importStatus.textContent = 'Erro de importação: O arquivo JSON não contém as chaves "students" e "users".';
                      performImportButton.disabled = true; // Disable import again on error
                      document.getElementById('importFileName').textContent = 'Nenhum arquivo selecionado.';
                      fileInput.value = ''; // Clear file input
                 }
             } catch (e) {
                 importStatus.textContent = 'Erro ao processar arquivo JSON: ' + e.message;
                  performImportButton.disabled = true; // Disable import again on error
                  document.getElementById('importFileName').textContent = 'Nenhum arquivo selecionado.';
                  fileInput.value = ''; // Clear file input
             } finally {
                 // Clear the file input regardless of success or failure for security/cleanup
                 fileInput.value = '';
             }
         };
         reader.readAsText(file); // Read the file as text
         performImportButton.disabled = true; // Disable button after starting import
    }

    function deleteStudentDiscipline(event) {
        const button = event.target; const studentId = button.dataset.studentId; const disciplineName = button.dataset.disciplineName; const unitNumber = parseInt(button.dataset.unitNumber);
        const student = students.find(s => s.id === studentId); if (!student) { alert('Erro: Aluno não encontrado para exclusão da disciplina.'); return; }
        const disciplineIndex = student.disciplines.findIndex(d => d.discipline === disciplineName && d.unit === unitNumber);
        if (disciplineIndex === -1) { alert('Erro: Entrada de disciplina não encontrada para exclusão.'); return; }
        if (confirm(`Tem certeza que deseja excluir a disciplina "${disciplineName}" (Unidade ${unitNumber}) para o aluno "${student.name}"? Esta ação não pode ser desfeita."`)) {
            student.disciplines.splice(disciplineIndex, 1);
            saveData(); // Save data after deleting a discipline
            renderStudentTable(students);
            if (currentUser && currentUser.role === 'professor') loadProfessorTable();
            if (currentUser && currentUser.role === 'aluno' && currentUser.studentId === student.id) showStudentBulletin();
            alert('Disciplina excluída com sucesso.');
        }
    }
    function attachDeleteButtonListeners(tableSelector) {
        const table = document.querySelector(tableSelector); if (!table) return;
        const tableBody = table.querySelector('tbody');
        if (tableBody) { tableBody.removeEventListener('click', handleTableButtonClick); tableBody.addEventListener('click', handleTableButtonClick); }
    }
    function handleTableButtonClick(event) { if (event.target.classList.contains('delete-button')) deleteStudentDiscipline(event); }
    document.getElementById('importFile').addEventListener('change', function(event) {
        const fileNameSpan = document.getElementById('importFileName'); const performImportButton = document.getElementById('performImportButton'); const file = event.target.files[0];
        if (file) { fileNameSpan.textContent = file.name; performImportButton.disabled = false; } else { fileNameSpan.textContent = 'Nenhum arquivo selecionado.'; performImportButton.disabled = true; }
        document.getElementById('importStatus').textContent = '';
    });
    const allClasses = ["1A", "1B", "1C", "2A", "2B", "3A"];
     const allDisciplines = ["Redação", "Gramática", "Educação Física", "Literatura", "Geografia", "Inglês", "História", "Projeto de Vida", "Artes", "Matemática", "Filosofia", "Física", "Química", "Biologia", "Formação Profissional", "Inovaê", "Sociologia"];

     // New function to populate the discipline and class checkboxes for professor assignment
    function populateProfessorAssignmentCheckboxes(containerElement, assignedAssignments = []) {
        containerElement.innerHTML = ''; // Clear previous content

        allDisciplines.forEach(discipline => {
            const disciplineDiv = document.createElement('div');
            disciplineDiv.classList.add('discipline-assignment');
            disciplineDiv.dataset.discipline = discipline; // Store discipline name for easy access

            const disciplineTitle = document.createElement('h4');
            disciplineTitle.textContent = discipline;
            disciplineDiv.appendChild(disciplineTitle);

            const classesDiv = document.createElement('div');
            classesDiv.classList.add('class-checkboxes');

            allClasses.forEach(className => {
                const label = document.createElement('label');
                const checkbox = document.createElement('input');
                checkbox.type = 'checkbox';
                checkbox.value = className;
                // Check if this discipline in this class is already assigned
                const isAssigned = assignedAssignments.some(assignment =>
                    assignment.discipline === discipline && assignment.classes.includes(className)
                );
                if (isAssigned) {
                    checkbox.checked = true;
                }

                label.appendChild(checkbox);
                label.appendChild(document.createTextNode(className));
                classesDiv.appendChild(label);
            });

            disciplineDiv.appendChild(classesDiv);
            containerElement.appendChild(disciplineDiv);
        });
    }


    document.getElementById('disciplineClassSelect').addEventListener('change', function() { populateStudentSelectForDiscipline(this.value); });
    document.getElementById('printClassSelect').addEventListener('change', function() { populateStudentPrintSelect(this.value); });
    function showAddAlunoUserForm() { const turmaSelect = document.getElementById('newAlunoTurmaSelect'); populateClassSelect(turmaSelect); document.getElementById('newAlunoNameInput').value = ''; document.getElementById('newAlunoMatriculaInput').value = ''; document.getElementById('newAlunoSenhaInput').value = ''; document.getElementById('newAlunoTurmaSelect').value = ''; document.getElementById('addAlunoUserModal').classList.remove('hidden'); }
    function saveAlunoUser() {
        const name = document.getElementById('newAlunoNameInput').value.trim(); const matricula = document.getElementById('newAlunoMatriculaInput').value.trim(); const senha = document.getElementById('newAlunoSenhaInput').value; const turma = document.getElementById('newAlunoTurmaSelect').value;
        if (!name || !matricula || !senha || !turma) { alert('Por favor, preencha Nome, Matrícula, Senha e selecione a Turma para o aluno.'); return; }
        if (users.some(user => user.username === matricula)) { alert(`Nome de usuário (matrícula) "${matricula}" já existe. Por favor, use uma matrícula única.`); return; }
        const correspondingStudents = students.filter(student => student.name.trim().toLowerCase() === name.trim().toLowerCase() && student.class === turma);
        let studentIdToLink = null;
        if (correspondingStudents.length === 0) { alert(`Erro: Nenhum aluno encontrado com o nome "${name}" na turma "${turma}". O usuário não foi criado.`); return; }
        else if (correspondingStudents.length > 1) { alert(`Erro: Múltiplos alunos encontrados com o nome "${name}" na turma "${turma}". Não é possível criar o usuário.`); return; }
        else { studentIdToLink = correspondingStudents[0].id; }
        users.push({ username: matricula, password: senha, role: 'aluno', name: name, studentId: studentIdToLink, assignments: [] }); // Aluno users have empty assignments
        saveData(); // Save data after adding an aluno user
        closeAddAlunoUserModal(); renderUsersTable(users); alert('Usuário de aluno adicionado com sucesso!');
    }
    function closeAddAlunoUserModal() { document.getElementById('addAlunoUserModal').classList.add('hidden'); document.getElementById('newAlunoNameInput').value = ''; document.getElementById('newAlunoMatriculaInput').value = ''; document.getElementById('newAlunoSenhaInput').value = ''; document.getElementById('newAlunoTurmaSelect').value = ''; }

    // MODIFICADA: printMyBulletin para usar seleções do aluno
    function printMyBulletin() {
        if (!currentUser || currentUser.role !== 'aluno') {
            alert('Esta função está disponível apenas para usuários aluno logados.');
            return;
        }
        const student = students.find(s => s.id === currentUser.studentId);
        if (!student) {
            alert('Não foi possível encontrar seus dados de boletim para impressão.');
            return;
        }

        // Pegar unidades selecionadas pelo aluno
        const selectedUnits = Array.from(document.querySelectorAll('#studentPrintOptions .studentUnitCheckboxPrint:checked')).map(cb => parseInt(cb.value));
        // Pegar tipo de relatório selecionado pelo aluno
        const reportType = document.querySelector('#studentPrintOptions input[name="studentReportType"]:checked').value;

        if (selectedUnits.length === 0) {
            alert('Por favor, selecione pelo menos uma unidade para imprimir.');
            return;
        }

        const bulletinHTML = generateBulletinHTML(student, selectedUnits, reportType, true); // Pass true to include notes
        openPrintWindow(bulletinHTML, `Boletim de ${student.name}`);
    }

    // NEW: Functions for Professor Notes Section

    function populateProfessorNotesControls() {
        const notesDisciplineSelect = document.getElementById('notesDisciplineSelect');
        const notesClassSelect = document.getElementById('notesClassSelect');
        const notesStudentSelect = document.getElementById('notesStudentSelect');
        const notesUnitSelect = document.getElementById('notesUnitSelect');

        notesDisciplineSelect.innerHTML = '<option value="">Selecione a Disciplina</option>';
        notesClassSelect.innerHTML = '<option value="">Selecione a Turma</option>';
        notesStudentSelect.innerHTML = '<option value="">Selecione o Aluno</option>';
        notesClassSelect.disabled = true;
        notesStudentSelect.disabled = true;

        if (currentUser && currentUser.assignments) {
            const uniqueDisciplines = [...new Set(currentUser.assignments.map(a => a.discipline))].sort();
            uniqueDisciplines.forEach(discipline => {
                const option = document.createElement('option');
                option.value = discipline;
                option.textContent = discipline;
                notesDisciplineSelect.appendChild(option);
            });
        }

        notesDisciplineSelect.removeEventListener('change', updateNotesClassSelect);
        notesDisciplineSelect.addEventListener('change', updateNotesClassSelect);

        notesClassSelect.removeEventListener('change', updateNotesStudentSelect);
        notesClassSelect.addEventListener('change', updateNotesStudentSelect);

        notesStudentSelect.removeEventListener('change', loadProfessorStudentNotes);
        notesStudentSelect.addEventListener('change', loadProfessorStudentNotes);

        notesUnitSelect.removeEventListener('change', loadProfessorStudentNotes);
        notesUnitSelect.addEventListener('change', loadProfessorStudentNotes);
    }

    function updateNotesClassSelect() {
        const notesDisciplineSelect = document.getElementById('notesDisciplineSelect');
        const notesClassSelect = document.getElementById('notesClassSelect');
        const selectedDiscipline = notesDisciplineSelect.value;

        notesClassSelect.innerHTML = '<option value="">Selecione a Turma</option>';
        notesClassSelect.disabled = true;
        document.getElementById('notesStudentSelect').innerHTML = '<option value="">Selecione o Aluno</option>';
        document.getElementById('notesStudentSelect').disabled = true;
        document.getElementById('professorNoteContent').value = '';
        document.getElementById('existingProfessorNotes').innerHTML = '<p class="no-notes" style="text-align: center; font-style: italic;">Selecione aluno, disciplina e unidade para ver ou adicionar anotações.</p>';

        if (currentUser && currentUser.assignments && selectedDiscipline) {
            const assignment = currentUser.assignments.find(a => a.discipline === selectedDiscipline);
            if (assignment && assignment.classes.length > 0) {
                assignment.classes.sort().forEach(className => {
                    const option = document.createElement('option');
                    option.value = className;
                    option.textContent = className;
                    notesClassSelect.appendChild(option);
                });
                notesClassSelect.disabled = false;
            }
        }
    }

    function updateNotesStudentSelect() {
        const notesClassSelect = document.getElementById('notesClassSelect');
        const notesStudentSelect = document.getElementById('notesStudentSelect');
        const selectedClass = notesClassSelect.value;

        notesStudentSelect.innerHTML = '<option value="">Selecione o Aluno</option>';
        notesStudentSelect.disabled = true;
        document.getElementById('professorNoteContent').value = '';
        document.getElementById('existingProfessorNotes').innerHTML = '<p class="no-notes" style="text-align: center; font-style: italic;">Selecione aluno, disciplina e unidade para ver ou adicionar anotações.</p>';

        if (selectedClass) {
            const studentsInClass = students.filter(s => s.class === selectedClass);
            if (studentsInClass.length > 0) {
                studentsInClass.sort((a, b) => a.name.localeCompare(b.name)).forEach(student => {
                    const option = document.createElement('option');
                    option.value = student.id;
                    option.textContent = student.name;
                    notesStudentSelect.appendChild(option);
                });
                notesStudentSelect.disabled = false;
            }
        }
    }

    function loadProfessorStudentNotes() {
        const notesStudentSelect = document.getElementById('notesStudentSelect');
        const notesDisciplineSelect = document.getElementById('notesDisciplineSelect');
        const notesUnitSelect = document.getElementById('notesUnitSelect');
        const existingNotesDiv = document.getElementById('existingProfessorNotes');

        const studentId = notesStudentSelect.value;
        const discipline = notesDisciplineSelect.value;
        const unit = parseInt(notesUnitSelect.value);

        existingNotesDiv.innerHTML = '';
        if (!studentId || !discipline || !unit) {
            existingNotesDiv.innerHTML = '<p class="no-notes" style="text-align: center; font-style: italic;">Selecione aluno, disciplina e unidade para ver ou adicionar anotações.</p>';
            return;
        }

        const student = students.find(s => s.id === studentId);
        if (student && student.professorNotes) {
            const relevantNotes = student.professorNotes.filter(note =>
                note.discipline === discipline && note.unit === unit
            ).sort((a, b) => new Date(b.date) - new Date(a.date)); // Sort by date descending

            if (relevantNotes.length > 0) {
                relevantNotes.forEach((note, index) => {
                    const noteItem = document.createElement('div');
                    noteItem.classList.add('professor-note-item');
                    noteItem.innerHTML = `
                        <p><strong>Anotação:</strong> ${note.content}</p>
                        <span class="note-meta">Feita por: ${note.professorName} em ${new Date(note.date).toLocaleDateString()}</span>
                        <button class="delete-note-button" data-student-id="${student.id}" data-note-index="${index}">Excluir</button>
                    `;
                    existingNotesDiv.appendChild(noteItem);
                });
                attachDeleteNoteButtonListeners(); // Attach listeners for newly rendered buttons
            } else {
                existingNotesDiv.innerHTML = '<p class="no-notes" style="text-align: center; font-style: italic;">Nenhuma anotação existente para esta seleção.</p>';
            }
        } else {
            existingNotesDiv.innerHTML = '<p class="no-notes" style="text-align: center; font-style: italic;">Nenhuma anotação existente para esta seleção.</p>';
        }
    }

    function saveProfessorNote() {
        const notesStudentSelect = document.getElementById('notesStudentSelect');
        const notesDisciplineSelect = document.getElementById('notesDisciplineSelect');
        const notesUnitSelect = document.getElementById('notesUnitSelect');
        const professorNoteContent = document.getElementById('professorNoteContent');

        const studentId = notesStudentSelect.value;
        const discipline = notesDisciplineSelect.value;
        const unit = parseInt(notesUnitSelect.value);
        const content = professorNoteContent.value.trim();

        if (!studentId || !discipline || !unit || !content) {
            alert('Por favor, selecione o Aluno, Disciplina, Unidade e escreva o conteúdo da anotação.');
            return;
        }

        const student = students.find(s => s.id === studentId);
        if (student) {
            const newNote = {
                professorName: currentUser.name,
                discipline: discipline,
                unit: unit,
                date: new Date().toISOString(), // ISO string for easy storage and parsing
                content: content
            };
            student.professorNotes.push(newNote);
            saveData();
            professorNoteContent.value = ''; // Clear textarea
            loadProfessorStudentNotes(); // Reload notes display
            if (currentUser.role === 'aluno' && currentUser.studentId === student.id) showStudentBulletin(); // Update student view if it's their bulletin
            alert('Anotação salva com sucesso!');
        } else {
            alert('Aluno não encontrado para salvar anotação.');
        }
    }

    function deleteProfessorNote(studentId, noteIndex) {
        const student = students.find(s => s.id === studentId);
        if (student && student.professorNotes && student.professorNotes.length > noteIndex) {
            if (confirm('Tem certeza que deseja excluir esta anotação?')) {
                student.professorNotes.splice(noteIndex, 1);
                saveData();
                loadProfessorStudentNotes(); // Reload notes display
                if (currentUser.role === 'aluno' && currentUser.studentId === student.id) showStudentBulletin(); // Update student view if it's their bulletin
                alert('Anotação excluída.');
            }
        } else {
            alert('Erro: Anotação não encontrada.');
        }
    }

    function attachDeleteNoteButtonListeners() {
        document.querySelectorAll('.professor-note-item .delete-note-button').forEach(button => {
            button.removeEventListener('click', handleDeleteNoteClick); // Prevent multiple listeners
            button.addEventListener('click', handleDeleteNoteClick);
        });
    }

    function handleDeleteNoteClick(event) {
        const studentId = event.target.dataset.studentId;
        const noteIndex = parseInt(event.target.dataset.noteIndex);
        deleteProfessorNote(studentId, noteIndex);
    }

    // NEW: Function to display notes for the student in their bulletin
    function displayStudentNotes(student) {
        const studentNotesContent = document.getElementById('studentNotesContent');
        studentNotesContent.innerHTML = ''; // Clear existing content

        if (!student || !student.professorNotes || student.professorNotes.length === 0) {
            studentNotesContent.innerHTML = '<p class="no-notes">Nenhuma anotação disponível.</p>';
            return;
        }

        // Sort notes by date (descending), then discipline, then professor
        const sortedNotes = student.professorNotes.slice().sort((a, b) => {
            const dateA = new Date(a.date);
            const dateB = new Date(b.date);
            if (dateA.getTime() !== dateB.getTime()) return dateB.getTime() - dateA.getTime();
            if (a.discipline !== b.discipline) return a.discipline.localeCompare(b.discipline);
            return a.professorName.localeCompare(b.professorName);
        });

        sortedNotes.forEach(note => {
            const noteItem = document.createElement('div');
            noteItem.classList.add('professor-note-item');
            noteItem.innerHTML = `
                <p><strong>Disciplina:</strong> ${note.discipline}, <strong>Unidade:</strong> ${note.unit}</p>
                <p><strong>Anotação:</strong> ${note.content}</p>
                <span class="note-meta">Feita por: ${note.professorName} em ${new Date(note.date).toLocaleDateString()}</span>
            `;
            studentNotesContent.appendChild(noteItem);
        });
    }

</script>

<div class="modal hidden" id="addAlunoUserModal">
<div class="modal-content">
<span class="close-button" onclick="closeAddAlunoUserModal()">×</span>
<h2>Adicionar Usuário de Aluno</h2>
<div>
<label for="newAlunoNameInput">Nome do Aluno:</label>
<input id="newAlunoNameInput" placeholder="Nome completo do aluno" type="text"/>
</div>
<div>
<label for="newAlunoMatriculaInput">Matrícula:</label>
<input id="newAlunoMatriculaInput" placeholder="Matrícula do aluno" type="text"/>
</div>
<div>
<label for="newAlunoSenhaInput">Senha:</label>
<input id="newAlunoSenhaInput" placeholder="Senha para login do aluno" type="password"/>
</div>
<div>
<label for="newAlunoTurmaSelect">Turma:</label>
<select id="newAlunoTurmaSelect">
<option value="">Selecione a Turma</option>
<option value="1A">1A</option>
<option value="1B">1B</option>
<option value="1C">1C</option>
<option value="2A">2A</option>
<option value="2B">2B</option>
<option value="3A">3A</option>
</select>
</div>
<button class="button" onclick="saveAlunoUser()" type="button">Salvar Aluno</button>
</div>
</div>
</body>
</html>
