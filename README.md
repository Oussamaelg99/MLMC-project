
\documentclass{beamer}

% Set theme and colors
\usetheme{Madrid} % You can change the theme to your preference
\usecolortheme{seahorse} % You can change the color theme if desired

% Package for adding images and managing columns
\usepackage{graphicx}
\usepackage{multicol}

% Title Page Information
\title[Internship Defense]{\textbf{Title of Your Internship Project}}
\author{[Your Full Name]}
\institute{
  \vspace{0.5cm}
  \textbf{Institution Name/University Name} \\[0.5cm]
  Program Name \\[0.5cm]
  
  % Display the logos of the company and institutions side by side
  \begin{minipage}{0.3\textwidth}
      \centering
      \includegraphics[width=0.8\textwidth]{company_logo.png} % Path to the company logo
  \end{minipage}
  \begin{minipage}{0.3\textwidth}
      \centering
      \includegraphics[width=0.8\textwidth]{institution1_logo.png} % Path to the first institution logo
  \end{minipage}
  \begin{minipage}{0.3\textwidth}
      \centering
      \includegraphics[width=0.8\textwidth]{institution2_logo.png} % Path to the second institution logo
  \end{minipage}

  \vspace{1cm}
  
  % Supervisors and examiner section in two columns
  \begin{multicols}{2}
    \textbf{Academic Supervisor:} \\
    [Academic Supervisor's Name] \\
    \columnbreak
    \textbf{Examiner:} \\
    [Examiner's Name] \\
    \vspace{0.5cm}
    \textbf{Company Supervisor 1:} \\
    [Company Supervisor 1's Name] \\
    \columnbreak
    \textbf{Company Supervisor 2:} \\
    [Company Supervisor 2's Name]
  \end{multicols}
  
  \vspace{1cm}
  
  \textbf{Internship Period:} [Start Date] - [End Date] \\[0.5cm]
  \textbf{Defense Date:} [Defense Date]
}

\date{\today}

\begin{document}

% Title frame
\begin{frame}
  \titlepage
\end{frame}

% Additional content can go here...

\end{document}
