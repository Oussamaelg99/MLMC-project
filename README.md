\documentclass{report}
\usepackage{graphicx}  % Required for including images
\usepackage{lipsum}    % For placeholder text (you can remove this if not needed)

\begin{document}

% Create title page
\begin{titlepage}

    \centering
    % Company Logo at the top
    \includegraphics[width=0.3\textwidth]{company_logo.png} \par
    \vspace{1cm} % Adjust space between logo and title
    
    % Title of the report
    {\Huge \textbf{Your Report Title} \par}
    \vspace{2cm}
    
    % Company name
    {\LARGE Company Name \par}
    \vspace{2cm}
    
    % Tutor names
    {\Large Tutors: \par}
    {\large Tutor 1 Name \par}
    {\large Tutor 2 Name \par}
    \vspace{1cm}
    
    % Academic Tutor
    {\Large Academic Tutor: \par}
    {\large Academic Tutor Name \par}
    \vspace{1cm}
    
    % Examiner
    {\Large Examiner: \par}
    {\large Examiner Name \par}
    \vspace{1cm}
    
    % Your name
    {\Large Your Name \par}
    \vspace{2cm}
    
    % Logos of your two schools side by side
    \begin{minipage}{0.45\textwidth}
        \centering
        \includegraphics[width=0.8\textwidth]{school1_logo.png}
    \end{minipage}
    \hfill
    \begin{minipage}{0.45\textwidth}
        \centering
        \includegraphics[width=0.8\textwidth]{school2_logo.png}
    \end{minipage}
    
    \vfill % Adjust space to push content to the bottom

    % Date at the bottom
    {\large \today}
    
\end{titlepage}

\end{document}
