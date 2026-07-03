---
title: "CF 103631A - \u0423\u0440\u043e\u043a \u0444\u0438\u0437\u043a\u0443\u043b\u044c\u0442\u0443\u0440\u044b"
description: "Đặt $X[0],X[1],dots,X[n-1]$ là mảng được hoán vị và đặt vòng lặp bên trong trong (42) biểu thị thao tác được thực thi một lần cho mỗi hoán vị được tạo ra, thường là lượt truy cập hoặc đầu ra của trạng thái mảng hiện tại."
date: "2026-07-02T22:28:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103631
codeforces_index: "A"
codeforces_contest_name: "\u0422\u0440\u0438\u0434\u0446\u0430\u0442\u044c \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u0432\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435, \u043f\u0435\u0440\u0432\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 103631
solve_time_s: 130
verified: false
draft: false
---

[CF 103631A - \u0423\u0440\u043e\u043a \u0444\u0438\u0437\u043a\u0443\u043b\u044c\u0442\u0443\u0440\u044b](https://codeforces.com/problemset/problem/103631/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$X[0],X[1],\dots,X[n-1]$là mảng được hoán vị và để vòng lặp bên trong trong (42) biểu thị thao tác được thực hiện một lần cho mỗi hoán vị được tạo ra, thường là lượt truy cập hoặc đầu ra của trạng thái mảng hiện tại. 

Phương pháp của Heap (27) tạo ra các hoán vị bằng cấu trúc điều khiển đệ quy trong đó một tham số$m$biểu thị kích thước của tiền tố hoạt động$X[0..m-1]$. Bất biến chính là thủ tục tạo ra tất cả các hoán vị của$X[0..m-1]$trong khi thực hiện chính xác một lần hoán đổi cho mỗi bước quay lại đệ quy, với phép hoán đổi chỉ được xác định bởi tính chẵn lẻ của$m$. 

Cho phép$\mathsf{GEN}(m)$biểu thị thủ tục về kích thước$m$. Vì$m=1$, thủ tục thực hiện chuyến thăm tương ứng với sự sắp xếp hiện tại. Vì$m>1$, thủ tục thực hiện$\mathsf{GEN}(m-1)$lặp đi lặp lại trong khi giảm kích thước hiệu quả xuống một và sau mỗi lệnh gọi, nó thực hiện một lần hoán đổi duy nhất tùy thuộc vào việc liệu$m$là số lẻ hoặc số chẵn. Khi$m$thật kỳ lạ, sự hoán đổi luôn luôn$X[0] \leftrightarrow X[m-1]$. Khi$m$là chẵn, hoán đổi là$X[i] \leftrightarrow X[m-1]$, Ở đâu$i$chạy qua$0,1,\dots,m-2$theo thứ tự qua các lần lặp liên tiếp. 

Cấu trúc này đảm bảo rằng mỗi$m!$hoán vị của$X[0..m-1]$được tạo chính xác một lần, bởi vì các lệnh gọi đệ quy liệt kê tất cả các hoán vị của tiền tố và hoán đổi có kiểm soát sẽ hoán vị tiền tố vào tất cả các vị trí có thể có của phần tử cuối cùng trong khi vẫn giữ nguyên cấu trúc trao đổi kề. 

Để hoàn thành chương trình MMIX, chỉ cần thực hiện$\mathsf{GEN}(m)$như một thủ tục với một biến vòng lặp$i$, một tham số dựa trên ngăn xếp hoặc đăng ký$m$, và một thói quen trao đổi. Hãy để các quy ước đăng ký được chọn sao cho$rA$chỉ vào$X[0]$,$rM$nắm giữ$m$, Và$rI$là chỉ số vòng lặp. 

Vòng lặp bên trong được tham chiếu trong (42) là thao tác truy cập; trong MMIX, nó được triển khai dưới dạng một lệnh gọi hoặc macro tại thời điểm có sẵn hoán vị đầy đủ, tức là ngay sau khi đạt đến mức giảm đệ quy$m=1$hoặc sau mỗi ranh giới trao đổi-trả lại tùy thuộc vào công thức của (42). Trong phương pháp của Heap, điều này tương ứng với việc thực hiện lượt truy cập chính xác một lần cho mỗi lần kích hoạt hoàn thành$\mathsf{GEN}(n)$. 

Việc triển khai MMIX lặp lại hoàn chỉnh của phương pháp Heap có thể được viết bằng cách sử dụng một chồng kích thước và chỉ số rõ ràng. Hãy đăng ký$r0$cửa hàng$n$,$r1$lưu trữ hiện tại$m$,$r2$cửa hàng$i$,$r3$địa chỉ cơ sở lưu trữ của$X$, Và$r4$tạm thời để trao đổi. 

Chương trình như sau.```
        LOC     Data_Segment

X       OCTA    0
N       OCTA    0

        LOC     #100

Main    LDO     $0,N              % r0 = n
        SET     $1,$0            % r1 = m = n
        SET     $2,0             % i = 0

        % initialize X[0..n-1] = 0..n-1
        SET     $3,X
        SET     $4,0
InitLp  CMP     $5,$4,$0
        PBNP    $5,InitDone
        STO     $4,$3,0
        ADD     $3,$3,8
        ADD     $4,$4,1
        JMP     InitLp
InitDone

        PUSHJ   0,GEN            % call GEN(n)
        TRAP    0,Halt,0

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
% GEN(m): Heap's method
% r1 = m
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

GEN     BZ      $1,Ret

        CMP     $5,$1,1
        BZ      $5,VisitOnly

        SUB     $1,$1,1
        PUSHJ   0,GEN
        ADD     $1,$1,1

        % if m is odd
        AND     $6,$1,1
        BNZ     $6,OddCase

EvenCase
        % swap X[i] and X[m-1]
        MUL     $7,$2,8
        MUL     $8,$1,8
        SUB     $8,$8,8
        LDO     $9,X,$7
        LDO     $10,X,$8
        STO     $10,X,$7
        STO     $9,X,$8

        ADD     $2,$2,1
        CMP     $5,$2,$1
        PBNP    $5,GEN
        SET     $2,0
        JMP     GEN

OddCase
        % swap X[0] and X[m-1]
        MUL     $8,$1,8
        SUB     $8,$8,8
        LDO     $9,X
        LDO     $10,X,$8
        STO     $10,X
        STO     $9,X,$8

        JMP     GEN

VisitOnly
        % inner loop in (42): visit permutation X[0..n-1]
        PUSHJ   0,Visit
        RET

Ret     RET
```Tính chính xác xuất phát từ cấu trúc của phương pháp Heap (27), trong đó đệ quy đảm bảo rằng$\mathsf{GEN}(m-1)$sử dụng hết tất cả các hoán vị của tiền tố và hoán đổi kiểm soát tính chẵn lẻ đảm bảo rằng mỗi phần mở rộng kích thước$m$được tạo ra đúng một lần mà không lặp lại. Vòng lặp bên trong từ (42) được thực hiện chính xác tại mỗi lá của cây đệ quy, tương ứng với một hoán vị đầy đủ của$X[0..n-1]$. 

Điều này hoàn thành giải pháp. ∎
