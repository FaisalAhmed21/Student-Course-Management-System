.MODEL SMALL
.STACK 100H
.DATA  
    sign1       DB '********************************$'
    log_page    DB '         LOGIN PAGE$'
    ; Login prompts
    login_username  DB 'Enter Username: $'
    login_pass      DB 'Enter Password: $'
    wrong_username  DB 'Invalid Username or Password! Try Again$'
    ; Storage for user input
    name_       DB 8 DUP(?)    ; Array to store input username
    pass_       DB 5 DUP(?)    ; Array to store input password
    ; Predefined users and passwords
    advisor       DB '#advisor'     ; Advisor username
    advisorpass   DB '12345'        ; Advisor password
    student1       DB 'student1'    ; Student1 username
    student1pass   DB 'pass1'       ; Student1 password
    student2       DB 'student2'    ; Student2 username
    student2pass   DB 'pass2'       ; Student2 password
    student3       DB 'student3'    ; Student3 username
    student3pass   DB 'pass3'       ; Student3 password

    ; Student 1 Information
    st1_header     DB '==== Student 1 Information ====$'
    st1_name       DB 'Name: Faisal Ahmed$'
    st1_id         DB 'ID: 21301186'
    st1_cgpa       DB 'CGPA: 2.00$' 
    stu1_cgpa      DB 20
    st1_courses    DB 'Completed Courses: Programming, Database, Algorithms$'

    ; Student 2 Information
    st2_header     DB '==== Student 2 Information ====$'
    st2_name       DB 'Name: Shafiur Rahman$'
    st2_id         DB 'ID: 21201613'
    st2_cgpa       DB 'CGPA: 4.00$' 
    stu2_cgpa      DB 40
    st2_courses    DB 'Completed Courses: Web Dev, OS, Computer Networks$'

    ; Student 3 Information
    st3_header     DB '==== Student 3 Information ====$'
    st3_name       DB 'Name: Mahmuda Akter Alif$'
    st3_id         DB 'ID: 21201149'
    st3_cgpa       DB 'CGPA: 3.90$'
    stu3_cgpa      DB 39
    st3_courses    DB 'Completed Courses: AI, Machine Learning, Data Science$'  
    
    course_header    DB 'Available Courses for Registration:$'
    course1         DB '1. Data Structures$'
    course2         DB '2. Computer Architecture$'
    course3         DB '3. Microprocessor$'
    course4         DB '4. Digital Logic Design$'
    course5         DB '5. Software Engineering$'
    course6         DB '6. Computer Graphics$'
    course7         DB '7. Artificial Intelligence$' 
    selected_msg     DB 'Your Selected Courses:$'
    
    ; Course Registration Messages
    reg_prompt      DB 'Enter course number to register (0 to finish): $'
    max_course_msg  DB 'Maximum courses reached!$'
    done_msg        DB 'Registration Complete!$' 
    
    courses_header DB 'Available Courses and Seats:$'
    course_seats     DB 7 DUP(?)    ; Array to store available seats for each course
    initial_seats    DB 3, 4, 1, 6, 0, 5, 2  ; Initial seats for courses 1-7
    no_seats_msg     DB 'No seat available for this course!$'
    
    
    ; Variables for course selection
    selected_courses DB 5 DUP(0)  ; Array to store selected course numbers (0 means not selected)
    course_count    DB 0          ; Counter for number of courses selected
    invalid_msg     DB 'Course already selected! Try another.$'
    cgpa_limit_msg  DB 'CGPA below 3.5 - Maximum 4 courses allowed.$'
    
    advisor_menu    DB '1. View Student 1', 0DH, 0AH
                    DB '2. View Student 2', 0DH, 0AH
                    DB '3. View Student 3', 0DH, 0AH
                    DB 'Choose student (1-3): $'   
    add_only_prompt    DB 'A-Add course, R-Return: $'
    drop_done_prompt   DB 'D-Drop course, R-Return, F-Finish: $'
    invalid_course_msg DB 'Invalid course selection!$'
    current_courses    DB 'Current registered courses:$'
    modify_prompt   DB 'A-Add course, D-Drop course, R-Return: $'
    drop_prompt     DB 'Enter course number to drop: $' 
    
    
    adv_drop_msg DB 'Enter course number to drop (0 to cancel): $'
    adv_add_msg DB 'Enter course number to add (0 to cancel): $'
    adv_options DB 'A-Add course, D-Drop course, F-Finish: $'
    adv_initial_msg DB 'A-Add course, F-Finish: $'
    adv_invalid_drop DB 'Invalid course number to drop!$'
    adv_drop_success DB 'Course dropped successfully.$'
    adv_no_courses DB 'No courses selected yet.$'
    return_menu_msg DB 'Press any key to return to menu...$'
    
    
    total_fee_msg    DB 'Total Semester Fee: $'
    currency_msg     DB ' BDT$'
    fee_newline      DB 0DH, 0AH, '$'

    
    newline        DB 0DH, 0AH, '$'

.CODE
MAIN PROC
    ; Initialize DS
    MOV AX, @DATA
    MOV DS, AX  
    mov cx, 7          ; Loop counter for 7 courses
    mov si, 0 
    
init_seats:
    mov al, initial_seats[si]
    mov course_seats[si], al
    inc si
    loop init_seats
    mov ah, 2
    mov dl, 0dh
    int 21h
    mov dl, 0ah
    int 21h  
    mov ah, 2
    mov dl, 0dh
    int 21h
    mov dl, 0ah
    int 21h  
    mov ah, 9
    lea dx, sign1
    int 21h  
    mov ah, 2
    mov dl, 0dh
    int 21h
    mov dl, 0ah
    int 21h  
    mov ah, 9
    lea dx, log_page
    int 21h  
    mov ah, 2
    mov dl, 0dh
    int 21h
    mov dl, 0ah
    int 21h  
    mov ah, 9
    lea dx, sign1
    int 21h 
    jmp taking_input

; User input
againtry: 
    mov ah, 2
    mov dl, 0dh
    int 21h
    mov dl, 0ah
    int 21h  
    mov ah, 2
    mov dl, 0dh
    int 21h
    mov dl, 0ah
    int 21h  
    mov ah, 9
    lea dx, wrong_username
    int 21h  
    mov cx, 7                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          ; Counter for 7 courses
    mov si, 0          ; Source index  
    
taking_input:
    ; Taking username from user
    mov ah, 2
    mov dl, 0dh
    int 21h
    mov dl, 0ah
    int 21h  
    mov ah, 2
    mov dl, 0dh
    int 21h
    mov dl, 0ah
    int 21h  
    mov ah, 2
    mov dl, 0dh
    int 21h
    mov dl, 0ah
    int 21h
    mov ah, 9
    lea dx, login_username
    int 21h 
    mov cx, 8      ; Changed to 8 to match the array size in data segment
    mov si, 0 
    
take_name:
    mov ah, 1 
    int 21h 
    mov name_[si], al   ; Corrected variable name here
    inc si
    loop take_name    
    mov ah, 2
    mov dl, 0dh
    int 21h
    mov dl, 0ah
    int 21h  
    ; Taking password from the user
    mov ah, 9
    lea dx, login_pass
    int 21h  
    mov cx, 5
    mov si, 0 
    
takepass:
    mov ah, 1 
    int 21h 
    mov pass_[si], al
    inc si
    loop takepass
    mov ah, 2
    mov dl, 0dh
    int 21h
    mov dl, 0ah
    int 21h
 
    ; For advisor
check0:
    mov cx, 8      ; Advisor username length is 8
    mov si, 0  
    
aduser:
    mov al, advisor[si]
    mov bl, name_[si]
    cmp al, bl
    jne check1
    inc si
    loop aduser
    jmp advisor_passcheck    ; Changed from student1_passcheck

check1:
    mov cx, 8      ; Changed to 8 to match student usernames
    mov si, 0 
    
st1user:
    mov al, student1[si]
    mov bl, name_[si]
    cmp al, bl
    jne check2
    inc si
    loop st1user
    jmp student1_passcheck   ; This label is correct

check2: 
    mov cx, 8      ; Changed to 8 to match student usernames
    mov si, 0 
    
st2user:
    mov al, student2[si]
    mov bl, name_[si]
    cmp al, bl
    jne check3
    inc si
    loop st2user
    jmp student2_passcheck   ; This label is correct

check3: 
    mov cx, 8      ; Changed to 8 to match student usernames
    mov si, 0 
    
st3user:
    mov al, student3[si]
    mov bl, name_[si]
    cmp al, bl
    jne againtry
    inc si
    loop st3user
    jmp student3_passcheck   ; This label is correct

; For advisor
advisor_passcheck:           ; Added this section
    mov cx, 5
    mov si, 0 
    
adpass:
    mov al, advisorpass[si]
    mov bl, pass_[si]
    cmp al, bl
    jne againtry
    inc si
    loop adpass
    jmp advisor_panel

; For student1
student1_passcheck:         ; This label exists and is correct
    mov cx, 5
    mov si, 0  
    
st1pass:
    mov al, student1pass[si]
    mov bl, pass_[si]
    cmp al, bl
    jne againtry
    inc si
    loop st1pass
    jmp student1_panel

; For student2
student2_passcheck:         ; This label exists and is correct
    mov cx, 5
    mov si, 0
    
st2pass:
    mov al, student2pass[si]
    mov bl, pass_[si]
    cmp al, bl
    jne againtry
    inc si
    loop st2pass
    jmp student2_panel

; For student3
student3_passcheck:         ; This label exists and is correct
    mov cx, 5
    mov si, 0
st3pass:
    mov al, student3pass[si]
    mov bl, pass_[si]
    cmp al, bl
    jne againtry
    inc si
    loop st3pass
    jmp student3_panel
    
MAIN ENDP

advisor_panel PROC
    push ax
    push dx     
    
advisor_menu_start:    ; New label for returning to menu
    ; Clear screen
    mov ah, 0
    mov al, 3
    int 10h
    ; Display menu
    lea dx, advisor_menu
    mov ah, 9
    int 21h
    ; Get menu choice
    mov ah, 1
    int 21h
    sub al, '0'
    ; Check menu choice
    cmp al, 1
    je adv_view_student1
    cmp al, 2
    je adv_view_student2
    cmp al, 3
    je adv_view_student3
    jmp exit_panel

adv_view_student1:
    call display_student1_info
    call adv_manage_courses1
    jmp advisor_panel

adv_view_student2:
    call display_student2_info
    call adv_manage_courses2
    jmp advisor_panel

adv_view_student3:
    call display_student3_info
    call adv_manage_courses3
    jmp advisor_panel

exit_panel:
    pop dx
    pop ax
    ret 
    
advisor_panel ENDP

; Display procedures for each student
display_student1_info PROC
    push ax
    push dx
    lea dx, st1_header
    mov ah, 9
    int 21h
    lea dx, newline
    int 21h
    lea dx, st1_name
    int 21h
    lea dx, newline
    int 21h
    lea dx, st1_id
    int 21h
    lea dx, newline
    int 21h
    lea dx, st1_cgpa
    int 21h
    lea dx, newline
    int 21h
    lea dx, st1_courses
    int 21h
    lea dx, newline
    int 21h
    pop dx
    pop ax
    ret
    
display_student1_info ENDP

display_student2_info PROC
    push ax
    push dx
    lea dx, st2_header
    mov ah, 9
    int 21h
    lea dx, newline
    int 21h
    lea dx, st2_name
    int 21h
    lea dx, newline
    int 21h
    lea dx, st2_id
    int 21h
    lea dx, newline
    int 21h
    lea dx, st2_cgpa
    int 21h
    lea dx, newline
    int 21h
    lea dx, st2_courses
    int 21h
    lea dx, newline
    int 21h
    pop dx
    pop ax
    ret  
    
display_student2_info ENDP

display_student3_info PROC
    push ax
    push dx
    lea dx, st3_header
    mov ah, 9
    int 21h
    lea dx, newline
    int 21h
    lea dx, st3_name
    int 21h
    lea dx, newline
    int 21h
    lea dx, st3_id
    int 21h
    lea dx, newline
    int 21h
    lea dx, st3_cgpa
    int 21h
    lea dx, newline
    int 21h
    lea dx, st3_courses
    int 21h
    lea dx, newline
    int 21h
    pop dx
    pop ax
    ret
    
display_student3_info ENDP


; Course management procedures
adv_manage_courses1 PROC
    push ax
    push bx
    push cx
    push dx
    push si
    ; Reset course count
    mov course_count, 0
    ; Display available courses
    call display_available_courses
    ; Start with empty course list
    cmp course_count, 0
    je first_add
    
show_options:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_options
    int 21h
    mov ah, 1
    int 21h
    cmp al, 'A'
    je add_course
    cmp al, 'D'
    je drop_course
    cmp al, 'F'
    je finish
    jmp show_options

first_add:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_initial_msg
    int 21h
    mov ah, 1
    int 21h
    cmp al, 'A'
    je add_course
    cmp al, 'F'
    je finish
    jmp first_add

add_course:
    ; Check if max courses reached based on CGPA
    mov al, course_count
    cmp al, 4
    jge check_cgpa_limit
    jmp do_add

check_cgpa_limit:
    mov al, stu1_cgpa
    cmp al, 35          ; Compare with 3.5 (stored as 35)
    jl max_reached      ; If CGPA < 3.5, don't allow more courses
    cmp course_count, 5
    jge max_reached
    
do_add:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_add_msg
    int 21h
    mov ah, 1
    int 21h
    sub al, '0'
    cmp al, 0
    je show_options
    ; Validate course number
    cmp al, 1
    jl do_add
    cmp al, 7
    jg do_add
    ; Check for duplicate
    push ax
    mov cx, 5
    mov si, 0  
    
check_dup:
    cmp selected_courses[si], al
    je dup_found
    inc si
    loop check_dup
    pop ax
    mov bh, 0
    mov bl, course_count
    mov si, bx
    mov selected_courses[si], al
    inc course_count
    call display_selected_courses
    jmp show_options

dup_found:
    pop ax
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, invalid_msg
    int 21h
    jmp show_options

drop_course:
    cmp course_count, 0
    je no_courses
    call display_selected_courses
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_drop_msg
    int 21h
    mov ah, 1
    int 21h
    sub al, '0'
    cmp al, 0
    je show_options
    ; Find and remove course
    mov cx, 5
    mov si, 0
    
find_course:
    cmp selected_courses[si], al
    je remove_course
    inc si
    loop find_course
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_invalid_drop
    int 21h
    jmp show_options

remove_course:
    ; Shift remaining courses
    push si
    mov bx, si
    
shift_courses:
    mov al, selected_courses[si + 1]
    mov selected_courses[si], al
    inc si
    cmp si, 4
    jl shift_courses
    mov selected_courses[4], 0
    dec course_count
    pop si
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_drop_success
    int 21h
    call display_selected_courses
    jmp show_options

no_courses:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_no_courses
    int 21h
    jmp show_options

max_reached:
    lea dx, newline
    mov ah, 9
    int 21h
    mov al, stu1_cgpa
    cmp al, 35
    jge show_max
    lea dx, cgpa_limit_msg
    jmp display_max
    
show_max:
    lea dx, max_course_msg

display_max:
    mov ah, 9
    int 21h
    jmp show_options

finish:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, done_msg
    mov ah, 9
    int 21h
    ; Display final course selection
    call display_selected_courses
    ; Add return to menu option
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, newline
    int 21h
    ; Press any key to return to menu message
    lea dx, return_menu_msg    
    mov ah, 9
    int 21h
    ; Wait for key press
    mov ah, 1
    int 21h
    pop si
    pop dx
    pop cx
    pop bx
    pop ax
    ret
    
adv_manage_courses1 ENDP  

adv_manage_courses2 PROC
    push ax
    push bx
    push cx
    push dx
    push si
    ; Reset course count
    mov course_count, 0
    ; Display available courses
    call display_available_courses
    ; Start with empty course list
    cmp course_count, 0
    je first_add2
    
show_options2:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_options
    int 21h
    mov ah, 1
    int 21h
    cmp al, 'A'
    je add_course2
    cmp al, 'D'
    je drop_course2
    cmp al, 'F'
    je finish2
    jmp show_options2

first_add2:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_initial_msg
    int 21h
    mov ah, 1
    int 21h
    cmp al, 'A'
    je add_course2
    cmp al, 'F'
    je finish2
    jmp first_add2

add_course2:
    ; Check if max courses reached
    mov al, course_count
    cmp al, 4
    jge check_cgpa2_adv
    jmp do_add2

check_cgpa2_adv:
    mov al, stu2_cgpa
    cmp al, 35
    jl max_reached2_adv
    cmp course_count, 5
    jge max_reached2_adv
    
do_add2:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_add_msg
    int 21h
    mov ah, 1
    int 21h
    sub al, '0'
    cmp al, 0
    je show_options2
    ; Validate course number
    cmp al, 1
    jl do_add2
    cmp al, 7
    jg do_add2
    ; Check for duplicate
    push ax
    mov cx, 5
    mov si, 0 
    
check_dup2:
    cmp selected_courses[si], al
    je dup_found2
    inc si
    loop check_dup2
    pop ax
    mov bh, 0
    mov bl, course_count
    mov si, bx
    mov selected_courses[si], al
    inc course_count
    call display_selected_courses
    jmp show_options2

dup_found2:
    pop ax
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, invalid_msg
    int 21h
    jmp show_options2

drop_course2:
    cmp course_count, 0
    je no_courses2
    call display_selected_courses
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_drop_msg
    int 21h
    mov ah, 1
    int 21h
    sub al, '0'
    cmp al, 0
    je show_options2
    ; Find and remove course
    mov cx, 5
    mov si, 0 
    
find_course2:
    cmp selected_courses[si], al
    je remove_course2
    inc si
    loop find_course2
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_invalid_drop
    int 21h
    jmp show_options2

remove_course2:
    ; Shift remaining courses
    push si
    mov bx, si
shift_courses2:
    mov al, selected_courses[si + 1]
    mov selected_courses[si], al
    inc si
    cmp si, 4
    jl shift_courses2
    mov selected_courses[4], 0
    dec course_count
    pop si
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_drop_success
    int 21h
    call display_selected_courses
    jmp show_options2

no_courses2:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_no_courses
    int 21h
    jmp show_options2

max_reached2_adv:
    lea dx, newline
    mov ah, 9
    int 21h
    mov al, stu2_cgpa
    cmp al, 35
    jge show_max2
    lea dx, cgpa_limit_msg
    jmp display_max2
    
show_max2:
    lea dx, max_course_msg

display_max2:
    mov ah, 9
    int 21h
    jmp show_options2

finish2:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, done_msg
    mov ah, 9
    int 21h
    ; Display final course selection
    call display_selected_courses
    ; Add return to menu option
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, newline
    int 21h
    ; Press any key to return to menu message
    lea dx, return_menu_msg    
    mov ah, 9
    int 21h
    ; Wait for key press
    mov ah, 1
    int 21h
    pop si
    pop dx
    pop cx
    pop bx
    pop ax
    ret
    
adv_manage_courses2 ENDP

adv_manage_courses3 PROC
    push ax
    push bx
    push cx
    push dx
    push si
    ; Reset course count
    mov course_count, 0
    ; Display available courses
    call display_available_courses
    ; Start with empty course list
    cmp course_count, 0
    je first_add3
    
show_options3:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_options
    int 21h
    mov ah, 1
    int 21h
    cmp al, 'A'
    je add_course3
    cmp al, 'D'
    je drop_course3
    cmp al, 'F'
    je finish3
    jmp show_options3

first_add3:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_initial_msg
    int 21h
    mov ah, 1
    int 21h
    cmp al, 'A'
    je add_course3
    cmp al, 'F'
    je finish3
    jmp first_add3

add_course3:
    ; Check if max courses reached
    mov al, course_count
    cmp al, 4
    jge check_cgpa3_adv
    jmp do_add3

check_cgpa3_adv:
    mov al, stu3_cgpa
    cmp al, 35
    jl max_reached3_adv
    cmp course_count, 5
    jge max_reached3_adv
    
do_add3:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_add_msg
    int 21h
    mov ah, 1
    int 21h
    sub al, '0'
    cmp al, 0
    je show_options3
    ; Validate course number
    cmp al, 1
    jl do_add3
    cmp al, 7
    jg do_add3
    ; Check for duplicate
    push ax
    mov cx, 5
    mov si, 0
    
check_dup3:
    cmp selected_courses[si], al
    je dup_found3
    inc si
    loop check_dup3
    pop ax
    mov bh, 0
    mov bl, course_count
    mov si, bx
    mov selected_courses[si], al
    inc course_count
    call display_selected_courses
    jmp show_options3

dup_found3:
    pop ax
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, invalid_msg
    int 21h
    jmp show_options3

drop_course3:
    cmp course_count, 0
    je no_courses3
    call display_selected_courses
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_drop_msg
    int 21h
    mov ah, 1
    int 21h
    sub al, '0'
    cmp al, 0
    je show_options3
    ; Find and remove course
    mov cx, 5
    mov si, 0 
    
find_course3:
    cmp selected_courses[si], al
    je remove_course3
    inc si
    loop find_course3
    
    lea dx, newline
    mov ah, 9
    int 21h
    
    lea dx, adv_invalid_drop
    int 21h
    jmp show_options3

remove_course3:
    ; Shift remaining courses
    push si
    mov bx, si
shift_courses3:
    mov al, selected_courses[si + 1]
    mov selected_courses[si], al
    inc si
    cmp si, 4
    jl shift_courses3
    mov selected_courses[4], 0
    dec course_count
    pop si
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, adv_drop_success
    int 21h
    call display_selected_courses
    jmp show_options3

no_courses3:
    lea dx, newline
    mov ah, 9
    int 21h
    
    lea dx, adv_no_courses
    int 21h
    jmp show_options3

max_reached3_adv:
    lea dx, newline
    mov ah, 9
    int 21h
    mov al, stu3_cgpa
    cmp al, 35
    jge show_max3
    lea dx, cgpa_limit_msg
    jmp display_max3
    
show_max3:
    lea dx, max_course_msg

display_max3:
    mov ah, 9
    int 21h
    jmp show_options3

finish3:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, done_msg
    mov ah, 9
    int 21h
    
    ; Display final course selection
    call display_selected_courses
    
    ; Add return to menu option
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, newline
    int 21h
    
    ; Press any key to return to menu message
    lea dx, return_menu_msg    
    mov ah, 9
    int 21h
    
    ; Wait for key press
    mov ah, 1
    int 21h
    pop si
    pop dx
    pop cx
    pop bx
    pop ax
    ret
adv_manage_courses3 ENDP

; Support procedures
display_available_courses PROC
    ; First, display header for available courses
    lea dx, courses_header
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    
    ; Display Course 1 with seats
    lea dx, course1
    mov ah, 9
    int 21h
    
    ; Display available seats for course 1
    mov dl, ' '        ; Space after course name
    mov ah, 2
    int 21h
    mov dl, '['        ; Opening bracket
    mov ah, 2
    int 21h
    mov al, course_seats[0]  ; Get number of seats
    add al, '0'             ; Convert to ASCII
    mov dl, al
    mov ah, 2
    int 21h
    mov dl, ']'        ; Closing bracket
    mov ah, 2
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    
    ; Display Course 2 with seats
    lea dx, course2
    mov ah, 9
    int 21h
    mov dl, ' '
    mov ah, 2
    int 21h
    mov dl, '['
    mov ah, 2
    int 21h
    mov al, course_seats[1]
    add al, '0'
    mov dl, al
    mov ah, 2
    int 21h
    mov dl, ']'
    mov ah, 2
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    
    ; Display Course 3 with seats
    lea dx, course3
    mov ah, 9
    int 21h
    mov dl, ' '
    mov ah, 2
    int 21h
    mov dl, '['
    mov ah, 2
    int 21h
    mov al, course_seats[2]
    add al, '0'
    mov dl, al
    mov ah, 2
    int 21h
    mov dl, ']'
    mov ah, 2
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    
    ; Display Course 4 with seats
    lea dx, course4
    mov ah, 9
    int 21h
    mov dl, ' '
    mov ah, 2
    int 21h
    mov dl, '['
    mov ah, 2
    int 21h
    mov al, course_seats[3]
    add al, '0'
    mov dl, al
    mov ah, 2
    int 21h
    mov dl, ']'
    mov ah, 2
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    
    ; Display Course 5 with seats
    lea dx, course5
    mov ah, 9
    int 21h
    mov dl, ' '
    mov ah, 2
    int 21h
    mov dl, '['
    mov ah, 2
    int 21h
    mov al, course_seats[4]
    add al, '0'
    mov dl, al
    mov ah, 2
    int 21h
    mov dl, ']'
    mov ah, 2
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    
    ; Display Course 6 with seats
    lea dx, course6
    mov ah, 9
    int 21h
    mov dl, ' '
    mov ah, 2
    int 21h
    mov dl, '['
    mov ah, 2
    int 21h
    mov al, course_seats[5]
    add al, '0'
    mov dl, al
    mov ah, 2
    int 21h
    
    mov dl, ']'
    mov ah, 2
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    
    ; Display Course 7 with seats
    lea dx, course7
    mov ah, 9
    int 21h
    mov dl, ' '
    mov ah, 2
    int 21h
    mov dl, '['
    mov ah, 2
    int 21h
    mov al, course_seats[6]
    add al, '0'
    mov dl, al
    mov ah, 2
    int 21h
    mov dl, ']'
    mov ah, 2
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    ret
    
display_available_courses ENDP

display_selected_courses PROC
    push ax
    push cx
    push dx
    push si
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, selected_msg
    int 21h
    lea dx, newline
    int 21h
    mov cx, 5
    mov si, 0 
    
display_loop:
    mov al, selected_courses[si]
    cmp al, 0
    je next_course
    push cx
    push si
    cmp al, 1
    je show1
    cmp al, 2
    je show2
    cmp al, 3
    je show3
    cmp al, 4
    je show4
    cmp al, 5
    je show5
    cmp al, 6
    je show6
    cmp al, 7
    je show7
    pop si
    pop cx 
    
next_course:
    inc si
    loop display_loop
    jmp display_done
    
show1:
    lea dx, course1
    jmp show_common
show2:
    lea dx, course2
    jmp show_common
show3:
    lea dx, course3
    jmp show_common
show4:
    lea dx, course4
    jmp show_common
show5:
    lea dx, course5
    jmp show_common
show6:
    lea dx, course6
    jmp show_common
show7:
    lea dx, course7
    
show_common:
    mov ah, 9
    int 21h
    lea dx, newline
    int 21h
    pop si
    pop cx
    jmp next_course

display_done:
    pop si
    pop dx
    pop cx
    pop ax
    ret
display_selected_courses ENDP 

display_semester_fee PROC
    push ax
    push bx
    push cx
    push dx
    ; Display newline for spacing
    lea dx, fee_newline
    mov ah, 9
    int 21h
    ; Calculate and display total fee
    lea dx, total_fee_msg
    int 21h
    ; Use 16-bit multiplication for total fee
    xor ax, ax              ; Clear AX
    mov al, course_count    ; Load course count
    mov bx, 198             ; Multiply by 198000 (showing in hundreds)
    mul bx                 ; Result in AX
    add ax, 104             ; Add 10400 (shown as 80 hundreds)
    call display_number
    mov dl, '0'           ; Add two zeros for hundreds
    mov ah, 2
    int 21h
    int 21h
    lea dx, currency_msg   ; Display currency
    mov ah, 9
    int 21h
    lea dx, fee_newline
    int 21h
    pop dx
    pop cx
    pop bx
    pop ax
    ret  
    
display_semester_fee ENDP

; Helper procedure to display numbers (unchanged)
display_number PROC
    push ax
    push bx
    push cx
    push dx
    mov cx, 0       ; Initialize digit counter
    mov bx, 10      ; Divisor 
    
convert_loop:
    mov dx, 0       ; Clear DX for division
    div bx          ; Divide AX by 10
    push dx         ; Save remainder (digit)
    inc cx          ; Increment digit counter
    cmp ax, 0       ; Check if quotient is 0
    jne convert_loop ; If not, continue loop  
    
disp_loop:
    pop dx          ; Get digit
    add dl, '0'     ; Convert to ASCII
    mov ah, 2       ; Display character
    int 21h
    loop disp_loop
    pop dx
    pop cx
    pop bx
    pop ax
    ret      
    
display_number ENDP

student1_panel PROC
    ; Clear screen first
    mov ah, 0
    mov al, 3
    int 10h
    ; Display Student 1 information
    lea dx, st1_header
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, st1_name
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, st1_id
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, st1_cgpa
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, st1_courses
    mov ah, 9
    int 21h 
    lea dx, newline
    mov ah, 9
    int 21h
    ; Check CGPA for course limit
    mov course_count, 0    ; Reset course counter
    ; Display available courses
    call display_available_courses   
    
take_courses1:
    mov al, course_count
    cmp al, 4      ; First check for 4 courses
    jge check_cgpa1
    jmp continue_reg1

check_cgpa1:
    mov al, stu1_cgpa    ; Load Student 1's CGPA
    cmp al, 35           ; Compare with 3.5 (stored as 35)
    jl max_reached1      ; If CGPA < 3.5, don't allow 5th course
    cmp course_count, 5  ; Check if already at max courses
    jge max_reached1     ; If at 5 courses, show max reached message
    jmp continue_reg1    ; Otherwise continue registration

continue_reg1:
    ; Prompt for course selection
    lea dx, reg_prompt
    mov ah, 9
    int 21h
    ; Get course number
    mov ah, 1
    int 21h
    sub al, '0'    ; Convert ASCII to number
    ; Check if done (0 entered)
    cmp al, 0
    je registration_done1
    ; Validate course number (1-7)
    cmp al, 1
    jl continue_reg1  ; Invalid course, ask again
    cmp al, 7
    jg continue_reg1  ; Invalid course, ask again
    ; Check if seats available
    push ax         ; Save course number
    dec al          ; Convert to 0-based index
    mov bx, 0
    mov bl, al      ; Move index to bx
    mov al, course_seats[bx]  ; Get number of seats
    cmp al, 0       ; Check if seats available
    jle no_seats1   ; Jump if no seats
    pop ax          ; Restore course number
    ; Check if course already selected
    mov cx, 5
    mov si, 0
    
check_duplicate1:
    cmp selected_courses[si], al
    je course_duplicate1
    inc si
    loop check_duplicate1
    ; Store course number if not duplicate
    mov bh, 0
    mov bl, course_count
    mov si, bx
    mov selected_courses[si], al
    ; Decrease available seats
    push ax
    dec al          ; Convert to 0-based index
    mov bx, 0
    mov bl, al      ; Move index to bx
    dec course_seats[bx]  ; Decrease seat count
    pop ax
    inc course_count
    lea dx, newline
    mov ah, 9
    int 21h
    jmp take_courses1

no_seats1:
    pop ax          ; Restore stack
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, no_seats_msg
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    jmp take_courses1

course_duplicate1:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, invalid_msg
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    jmp take_courses1  
    
max_reached1:
    lea dx, newline
    mov ah, 9
    int 21h
    mov al, stu1_cgpa
    cmp al, 35
    jge show_max_msg1
    lea dx, cgpa_limit_msg    ; Show CGPA limit message if CGPA < 3.5
    jmp display_limit_msg1
    
show_max_msg1:
    lea dx, max_course_msg    ; Show regular max course message if at 5 courses

display_limit_msg1:
    mov ah, 9
    int 21h
    jmp registration_done1

registration_done1:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, done_msg
    mov ah, 9
    int 21h  
    ; Display selected courses
    lea dx, selected_msg
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    ; Loop through selected courses and display them
    mov cx, 5          ; Maximum possible courses
    mov si, 0
display_courses1:
    mov al, selected_courses[si]
    cmp al, 0         ; Check if end of selections
    je finish_display1
    
    push cx           ; Save outer loop counter
    push si           ; Save current index
    
    ; Display course number and name based on selection
    cmp al, 1
    je display_course1
    cmp al, 2
    je display_course2
    cmp al, 3
    je display_course3
    cmp al, 4
    je display_course4
    cmp al, 5
    je display_course5
    cmp al, 6
    je display_course6
    cmp al, 7
    je display_course7
    pop si
    pop cx
    inc si
    loop display_courses1
    jmp finish_display1
    
display_course1:
    lea dx, course1
    jmp display_course_common
display_course2:
    lea dx, course2
    jmp display_course_common
display_course3:
    lea dx, course3
    jmp display_course_common
display_course4:
    lea dx, course4
    jmp display_course_common
display_course5:
    lea dx, course5
    jmp display_course_common
display_course6:
    lea dx, course6
    jmp display_course_common
display_course7:
    lea dx, course7
    jmp display_course_common

display_course_common:
    mov ah, 9
    int 21h
    lea dx, newline
    int 21h
    pop si
    pop cx
    inc si
    loop display_courses1

finish_display1:
    lea dx, done_msg
    mov ah, 9
    int 21h 
    call display_semester_fee
    jmp exit
    
student1_panel ENDP
    
student2_panel PROC
    ; Clear screen first
    mov ah, 0
    mov al, 3
    int 10h
    ; Display Student 2 information
    lea dx, st2_header
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, st2_name
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, st2_id
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, st2_cgpa
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, st2_courses
    mov ah, 9
    int 21h  
    lea dx, newline
    mov ah, 9
    int 21h
    ; Check CGPA for course limit
    mov course_count, 0    ; Reset course counter
    ; Display available courses
    call display_available_courses
    
take_courses2:
    mov al, course_count
    cmp al, 4      ; First check for 4 courses
    jge check_cgpa2
    jmp continue_reg2

check_cgpa2:
    mov al, stu2_cgpa    ; Load Student 2's CGPA
    cmp al, 35           ; Compare with 3.5 (stored as 35)
    jl max_reached2      ; If CGPA < 3.5, don't allow 5th course
    cmp course_count, 5  ; Check if already at max courses
    jge max_reached2     ; If at 5 courses, show max reached message
    jmp continue_reg2    ; Otherwise continue registration

continue_reg2:
    ; Prompt for course selection
    lea dx, reg_prompt
    mov ah, 9
    int 21h
    ; Get course number
    mov ah, 1
    int 21h
    sub al, '0'    ; Convert ASCII to number
    ; Check if done (0 entered)
    cmp al, 0
    je registration_done2
    ; Validate course number (1-7)
    cmp al, 1
    jl continue_reg2  ; Invalid course, ask again
    cmp al, 7
    jg continue_reg2  ; Invalid course, ask again
    ; Check if seats available
    push ax         ; Save course number
    dec al          ; Convert to 0-based index
    mov bx, 0
    mov bl, al      ; Move index to bx
    mov al, course_seats[bx]  ; Get number of seats
    cmp al, 0       ; Check if seats available
    jle no_seats2   ; Jump if no seats
    pop ax          ; Restore course number
    ; Check if course already selected
    mov cx, 5
    mov si, 0
    
check_duplicate2:
    cmp selected_courses[si], al
    je course_duplicate2
    inc si
    loop check_duplicate2
    ; Store course number if not duplicate
    mov bh, 0
    mov bl, course_count
    mov si, bx
    mov selected_courses[si], al
    ; Decrease available seats
    push ax
    dec al          ; Convert to 0-based index
    mov bx, 0
    mov bl, al      ; Move index to bx
    dec course_seats[bx]  ; Decrease seat count
    pop ax
    inc course_count
    lea dx, newline
    mov ah, 9
    int 21h
    jmp take_courses2 
    
no_seats2:
    pop ax          ; Restore stack
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, no_seats_msg
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    jmp take_courses2

course_duplicate2:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, invalid_msg
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    jmp take_courses2  
    
max_reached2:
    lea dx, newline
    mov ah, 9
    int 21h
    mov al, stu2_cgpa
    cmp al, 35
    jge show_max_msg2
    lea dx, cgpa_limit_msg    ; Show CGPA limit message if CGPA < 3.5
    jmp display_limit_msg2
    
show_max_msg2:
    lea dx, max_course_msg    ; Show regular max course message if at 5 courses

display_limit_msg2:
    mov ah, 9
    int 21h
    jmp registration_done2

registration_done2:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, done_msg
    mov ah, 9
    int 21h  
    ; Display selected courses
    lea dx, selected_msg
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    mov cx, 5          ; Maximum possible courses
    mov si, 0  
    
display_courses2:
    mov al, selected_courses[si]
    cmp al, 0         ; Check if end of selections
    je finish_display2
    push cx           ; Save outer loop counter
    push si           ; Save current index
    ; Display course number and name based on selection
    cmp al, 1
    je display_course1_2
    cmp al, 2
    je display_course2_2
    cmp al, 3
    je display_course3_2
    cmp al, 4
    je display_course4_2
    cmp al, 5
    je display_course5_2
    cmp al, 6
    je display_course6_2
    cmp al, 7
    je display_course7_2
    pop si
    pop cx
    inc si
    loop display_courses2
    jmp finish_display2
    
display_course1_2:
    lea dx, course1
    jmp display_course_common2
display_course2_2:
    lea dx, course2
    jmp display_course_common2
display_course3_2:
    lea dx, course3
    jmp display_course_common2
display_course4_2:
    lea dx, course4
    jmp display_course_common2
display_course5_2:
    lea dx, course5
    jmp display_course_common2
display_course6_2:
    lea dx, course6
    jmp display_course_common2
display_course7_2:
    lea dx, course7
    jmp display_course_common2

display_course_common2:
    mov ah, 9
    int 21h
    lea dx, newline
    int 21h
    pop si
    pop cx
    inc si
    loop display_courses2

finish_display2:
    lea dx, done_msg
    mov ah, 9
    int 21h
    call display_semester_fee
    jmp exit  
    
student2_panel ENDP 
   
student3_panel PROC
    ; Clear screen first
    mov ah, 0
    mov al, 3
    int 10h
    
    ; Display Student 3 information
    lea dx, st3_header
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, st3_name
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, st3_id
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, st3_cgpa
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, st3_courses
    mov ah, 9
    int 21h    
    lea dx, newline
    mov ah, 9
    int 21h
    ; Check CGPA for course limit
    mov course_count, 0    ; Reset course counter
    ; Display available courses
    call display_available_courses
    
take_courses3:
    mov al, course_count
    cmp al, 4      ; First check for 4 courses
    jge check_cgpa3
    jmp continue_reg3

check_cgpa3:
    mov al, stu3_cgpa    ; Load Student 3's CGPA
    cmp al, 35           ; Compare with 3.5 (stored as 35)
    jl max_reached3      ; If CGPA < 3.5, don't allow 5th course
    cmp course_count, 5  ; Check if already at max courses
    jge max_reached3     ; If at 5 courses, show max reached message
    jmp continue_reg3    ; Otherwise continue registration

continue_reg3:
    ; Prompt for course selection
    lea dx, reg_prompt
    mov ah, 9
    int 21h
    ; Get course number
    mov ah, 1
    int 21h
    sub al, '0'    ; Convert ASCII to number
    ; Check if done (0 entered)
    cmp al, 0
    je registration_done3
    ; Validate course number (1-7)
    cmp al, 1
    jl continue_reg3  ; Invalid course, ask again
    cmp al, 7
    jg continue_reg3  ; Invalid course, ask again
    ; Check if seats available
    push ax         ; Save course number
    dec al          ; Convert to 0-based index
    mov bx, 0
    mov bl, al      ; Move index to bx
    mov al, course_seats[bx]  ; Get number of seats
    cmp al, 0       ; Check if seats available
    jle no_seats3   ; Jump if no seats
    pop ax          ; Restore course number
    ; Check if course already selected
    mov cx, 5
    mov si, 0 
    
check_duplicate3:
    cmp selected_courses[si], al
    je course_duplicate3
    inc si
    loop check_duplicate3
    ; Store course number if not duplicate
    mov bh, 0
    mov bl, course_count
    mov si, bx
    mov selected_courses[si], al
    ; Decrease available seats
    push ax
    dec al          ; Convert to 0-based index
    mov bx, 0
    mov bl, al      ; Move index to bx
    dec course_seats[bx]  ; Decrease seat count
    pop ax
    inc course_count
    lea dx, newline
    mov ah, 9
    int 21h
    jmp take_courses3
    
no_seats3:
    pop ax          ; Restore stack
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, no_seats_msg
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    jmp take_courses3

course_duplicate3:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, invalid_msg
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    jmp take_courses3  
    
max_reached3:
    lea dx, newline
    mov ah, 9
    int 21h
    mov al, stu3_cgpa
    cmp al, 35
    jge show_max_msg3
    lea dx, cgpa_limit_msg    ; Show CGPA limit message if CGPA < 3.5
    jmp display_limit_msg3
    
show_max_msg3:
    lea dx, max_course_msg    ; Show regular max course message if at 5 courses

display_limit_msg3:
    mov ah, 9
    int 21h
    jmp registration_done3

registration_done3:
    lea dx, newline
    mov ah, 9
    int 21h
    lea dx, done_msg
    mov ah, 9
    int 21h  
    
    ; Display selected courses
    lea dx, selected_msg
    mov ah, 9
    int 21h
    lea dx, newline
    mov ah, 9
    int 21h
    mov cx, 5          ; Maximum possible courses
    mov si, 0  
    
display_courses3:
    mov al, selected_courses[si]
    cmp al, 0         ; Check if end of selections
    je finish_display3
    push cx           ; Save outer loop counter
    push si           ; Save current index
    ; Display course number and name based on selection
    cmp al, 1
    je display_course1_3
    cmp al, 2
    je display_course2_3
    cmp al, 3
    je display_course3_3
    cmp al, 4
    je display_course4_3
    cmp al, 5
    je display_course5_3
    cmp al, 6
    je display_course6_3
    cmp al, 7
    je display_course7_3
    pop si
    pop cx
    inc si
    loop display_courses3
    jmp finish_display3
    
display_course1_3:
    lea dx, course1
    jmp display_course_common3
display_course2_3:
    lea dx, course2
    jmp display_course_common3
display_course3_3:
    lea dx, course3
    jmp display_course_common3
display_course4_3:
    lea dx, course4
    jmp display_course_common3
display_course5_3:
    lea dx, course5
    jmp display_course_common3
display_course6_3:
    lea dx, course6
    jmp display_course_common3
display_course7_3:
    lea dx, course7
    jmp display_course_common3

display_course_common3:
    mov ah, 9
    int 21h
    lea dx, newline
    int 21h
    pop si
    pop cx
    inc si
    loop display_courses3

finish_display3:
    lea dx, done_msg
    mov ah, 9
    int 21h
    call display_semester_fee
    jmp exit 
    
student3_panel ENDP
    
exit:
    MOV AX, 4C00H
    INT 21H 
    
END MAIN